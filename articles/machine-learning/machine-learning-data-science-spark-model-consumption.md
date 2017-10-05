---
title: "Zprovoznit modely vytvořené Spark počítač learning | Microsoft Docs"
description: "Jak načíst a stanovíte jeho skóre learning modely uložené v Azure Blob Storage (WASB) s Python."
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
ms.openlocfilehash: 00fec675bed0137473f7e3c5ddfe9c3c0e8344c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="operationalize-spark-built-machine-learning-models"></a><span data-ttu-id="4b579-103">Zprovoznit learning modely vytvořené Spark počítače</span><span class="sxs-lookup"><span data-stu-id="4b579-103">Operationalize Spark-built machine learning models</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="4b579-104">Toto téma ukazuje, jak zprovoznit model uložené machine learning (ML) používá Python v clusterech HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-104">This topic shows how to operationalize a saved machine learning model (ML) using Python on HDInsight Spark clusters.</span></span> <span data-ttu-id="4b579-105">Popisuje jak načíst machine learning modely, které jste vytvořili pomocí Spark MLlib a uložené v Azure Blob Storage (WASB) a jak je skóre s datové sady, které byly uloženy také v WASB.</span><span class="sxs-lookup"><span data-stu-id="4b579-105">It describes how to load machine learning models that have been built using Spark MLlib and stored in Azure Blob Storage (WASB), and how to score them with datasets that have also been stored in WASB.</span></span> <span data-ttu-id="4b579-106">Zobrazuje postup předběžné zpracování vstupních dat, transformace funkcí s použitím funkce indexování a kódování v sadě nástrojů MLlib a jak vytvořit datový objekt s popiskem bod, který lze použít jako vstup pro vyhodnocování s modely ML.</span><span class="sxs-lookup"><span data-stu-id="4b579-106">It shows how to pre-process the input data, transform features using the indexing and encoding functions in the MLlib toolkit, and how to create a labeled point data object that can be used as input for scoring with the ML models.</span></span> <span data-ttu-id="4b579-107">Modely použité pro vyhodnocování zahrnují lineární regrese, Logistic Regression, náhodné modely doménové struktury a modely stromu přechodu zvyšovat skóre.</span><span class="sxs-lookup"><span data-stu-id="4b579-107">The models used for scoring include Linear Regression, Logistic Regression, Random Forest Models, and Gradient Boosting Tree Models.</span></span>

## <a name="spark-clusters-and-jupyter-notebooks"></a><span data-ttu-id="4b579-108">Clustery Spark a poznámkové bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="4b579-108">Spark clusters and Jupyter notebooks</span></span>
<span data-ttu-id="4b579-109">Kroky instalace a kód, který zprovoznit ML model jsou uvedené v tomto názorném postupu pro používání clusteru služby HDInsight Spark 1.6, jakož i cluster Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="4b579-109">Setup steps and the code to operationalize an ML model are provided in this walkthrough for using an HDInsight Spark 1.6 cluster as well as a Spark 2.0 cluster.</span></span> <span data-ttu-id="4b579-110">Kód pro tyto postupy je také součástí poznámkové bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4b579-110">The code for these procedures is also provided in Jupyter notebooks.</span></span>

### <a name="notebook-for-spark-16"></a><span data-ttu-id="4b579-111">Poznámkový blok pro Spark 1.6</span><span class="sxs-lookup"><span data-stu-id="4b579-111">Notebook for Spark 1.6</span></span>
<span data-ttu-id="4b579-112">[PySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Poznámkový blok Jupyter ukazuje, jak zprovoznit model uložené v clusterech prostředí HDInsight pomocí Pythonu.</span><span class="sxs-lookup"><span data-stu-id="4b579-112">The [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter notebook shows how to operationalize a saved model using Python on HDInsight clusters.</span></span> 

### <a name="notebook-for-spark-20"></a><span data-ttu-id="4b579-113">Poznámkový blok pro Spark 2.0</span><span class="sxs-lookup"><span data-stu-id="4b579-113">Notebook for Spark 2.0</span></span>
<span data-ttu-id="4b579-114">Pokud chcete upravit poznámkového bloku Jupyter pro 1.6 Spark pro použití s clusteru služby HDInsight Spark 2.0, nahradit soubor kód Python s [tento soubor](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span><span class="sxs-lookup"><span data-stu-id="4b579-114">To modify the Jupyter notebook for Spark 1.6 to use with an HDInsight Spark 2.0 cluster, replace the Python code file with [this file](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span></span> <span data-ttu-id="4b579-115">Tento kód ukazuje, jak využívat modelů vytvořených v Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="4b579-115">This code shows how to consume the models created in Spark 2.0.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="4b579-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b579-116">Prerequisites</span></span>

1. <span data-ttu-id="4b579-117">Budete potřebovat účet Azure a Spark 1.6 (nebo Spark 2.0) clusteru HDInsight k dokončení tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="4b579-117">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster to complete this walkthrough.</span></span> <span data-ttu-id="4b579-118">Najdete v článku [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny o tom, jak splnit tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="4b579-118">See the [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how to satisfy these requirements.</span></span> <span data-ttu-id="4b579-119">Toto téma obsahuje také popis NYC 2013 taxíkem data použít se zde a pokyny, jak provést kód z poznámkového bloku Jupyter v clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-119">That topic also contains a description of the NYC 2013 Taxi data used here and instructions on how to execute code from a Jupyter notebook on the Spark cluster.</span></span> 
2. <span data-ttu-id="4b579-120">Musíte taky vytvořit strojového učení modely, které se má vypočítat zde skóre projdete [zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) téma je určené pro cluster Spark 1.6 nebo poznámkových bloků Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="4b579-120">You must also create the machine learning models to be scored here by working through the [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic for the Spark 1.6 cluster or the Spark 2.0 notebooks.</span></span> 
3. <span data-ttu-id="4b579-121">Poznámkové bloky Spark 2.0 používat další datové sady pro úlohu klasifikace, dobře známé letecká společnost na čas odeslání datové sady z 2011 a 2012.</span><span class="sxs-lookup"><span data-stu-id="4b579-121">The Spark 2.0 notebooks use an additional data set for the classification task, the well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="4b579-122">Popis poznámkových bloků a odkazy na ně jsou součástí [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) úložiště Githubu, které je obsahují.</span><span class="sxs-lookup"><span data-stu-id="4b579-122">A description of the notebooks and links to them are provided in the [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for the GitHub repository containing them.</span></span> <span data-ttu-id="4b579-123">Kromě toho kód sem a v propojených poznámkových bloků je obecný a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-123">Moreover, the code here and in the linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="4b579-124">Pokud nepoužíváte HDInsight Spark, může být mírně lišit od co je tady uvedené kroky nastavení a Správa clusteru.</span><span class="sxs-lookup"><span data-stu-id="4b579-124">If you are not using HDInsight Spark, the cluster setup and management steps may be slightly different from what is shown here.</span></span> 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a><span data-ttu-id="4b579-125">Instalace: umístění úložiště, knihovny a kontext přednastavené Spark</span><span class="sxs-lookup"><span data-stu-id="4b579-125">Setup: storage locations, libraries, and the preset Spark context</span></span>
<span data-ttu-id="4b579-126">Spark se bude moct číst a zapisovat do Azure úložiště objektů Blob (WASB).</span><span class="sxs-lookup"><span data-stu-id="4b579-126">Spark is able to read and write to an Azure Storage Blob (WASB).</span></span> <span data-ttu-id="4b579-127">Takže existující data uložená existuje může zpracovat pomocí Spark a výsledky uložené v WASB znovu.</span><span class="sxs-lookup"><span data-stu-id="4b579-127">So any of your existing data stored there can be processed using Spark and the results stored again in WASB.</span></span>

<span data-ttu-id="4b579-128">Cesta k uložení modely nebo souborů v WASB, je třeba zadat správně.</span><span class="sxs-lookup"><span data-stu-id="4b579-128">To save models or files in WASB, the path needs to be specified properly.</span></span> <span data-ttu-id="4b579-129">Výchozí kontejner, který je připojen ke clusteru Spark se může odkazovat pomocí cesty počínaje: *"wasb / /"*.</span><span class="sxs-lookup"><span data-stu-id="4b579-129">The default container attached to the Spark cluster can be referenced using a path beginning with: *"wasb//"*.</span></span> <span data-ttu-id="4b579-130">Následující příklad kódu určuje umístění dat ke čtení a cesty k adresáři modelu úložiště, kde je uložen výstupní modelu.</span><span class="sxs-lookup"><span data-stu-id="4b579-130">The following code sample specifies the location of the data to be read and the path for the model storage directory to which the model output is saved.</span></span> 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="4b579-131">Nastavení cesty adresáře pro umístění úložiště v WASB</span><span class="sxs-lookup"><span data-stu-id="4b579-131">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="4b579-132">Modely se ukládají do: "wasb: / / / uživatel/remoteuser/NYCTaxi/modely".</span><span class="sxs-lookup"><span data-stu-id="4b579-132">Models are saved in: "wasb:///user/remoteuser/NYCTaxi/Models".</span></span> <span data-ttu-id="4b579-133">Pokud tato cesta není správně nastavena, modely nenačtou pro vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="4b579-133">If this path is not set properly, models are not loaded for scoring.</span></span>

<span data-ttu-id="4b579-134">Scored výsledky byly uloženy v: "wasb: / / / uživatel/remoteuser/NYCTaxi/ScoredResults".</span><span class="sxs-lookup"><span data-stu-id="4b579-134">The scored results have been saved in: "wasb:///user/remoteuser/NYCTaxi/ScoredResults".</span></span> <span data-ttu-id="4b579-135">Pokud cesta ke složce není v pořádku, výsledky nejsou uloženy v této složce.</span><span class="sxs-lookup"><span data-stu-id="4b579-135">If the path to folder is incorrect, results are not saved in that folder.</span></span>   

> [!NOTE]
> <span data-ttu-id="4b579-136">Cesta k umístění souborů lze kopírovat a vložit do zástupných symbolů v tento kód z výstupu poslední buňky **machine-learning-data-science-spark-data-exploration-modeling.ipynb** poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="4b579-136">The file path locations can be copied and pasted into the placeholders in this code from the output of the last cell of the **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebook.</span></span>   
> 
> 

<span data-ttu-id="4b579-137">Tady je kód pro nastavení cesty k adresáři:</span><span class="sxs-lookup"><span data-stu-id="4b579-137">Here is the code to set directory paths:</span></span> 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="4b579-138">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-138">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-139">DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)</span><span class="sxs-lookup"><span data-stu-id="4b579-139">datetime.datetime(2016, 4, 25, 23, 56, 19, 229403)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="4b579-140">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="4b579-140">Import libraries</span></span>
<span data-ttu-id="4b579-141">Nastavit kontext spark a importovat potřebné knihovny s následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="4b579-141">Set spark context and import necessary libraries with the following code</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="4b579-142">Předvolby kontextu Spark a Magic PySpark</span><span class="sxs-lookup"><span data-stu-id="4b579-142">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="4b579-143">Jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu.</span><span class="sxs-lookup"><span data-stu-id="4b579-143">The PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="4b579-144">Proto není potřeba nastavit Spark nebo vývoji Hive kontexty explicitně před zahájením práce s aplikací.</span><span class="sxs-lookup"><span data-stu-id="4b579-144">So you do not need to set the Spark or Hive contexts explicitly before you start working with the application you are developing.</span></span> <span data-ttu-id="4b579-145">Tyto jsou dostupné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4b579-145">These are available for you by default.</span></span> <span data-ttu-id="4b579-146">Tyto kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="4b579-146">These contexts are:</span></span>

* <span data-ttu-id="4b579-147">sc - pro Spark</span><span class="sxs-lookup"><span data-stu-id="4b579-147">sc - for Spark</span></span> 
* <span data-ttu-id="4b579-148">sqlContext - pro Hive</span><span class="sxs-lookup"><span data-stu-id="4b579-148">sqlContext - for Hive</span></span>

<span data-ttu-id="4b579-149">Poskytuje jádra PySpark některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%.</span><span class="sxs-lookup"><span data-stu-id="4b579-149">The PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="4b579-150">Existují dva takové příkazy, které se používají v tyto ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="4b579-150">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="4b579-151">**%% místní** zadat, že kód v další řádek se spustí místně.</span><span class="sxs-lookup"><span data-stu-id="4b579-151">**%%local** Specified that the code in subsequent lines is executed locally.</span></span> <span data-ttu-id="4b579-152">Kód musí být platný kód Python.</span><span class="sxs-lookup"><span data-stu-id="4b579-152">Code must be valid Python code.</span></span>
* <span data-ttu-id="4b579-153">**%% sql -o<variable name>**</span><span class="sxs-lookup"><span data-stu-id="4b579-153">**%%sql -o <variable name>**</span></span> 
* <span data-ttu-id="4b579-154">Provede dotaz Hive proti sqlContext.</span><span class="sxs-lookup"><span data-stu-id="4b579-154">Executes a Hive query against the sqlContext.</span></span> <span data-ttu-id="4b579-155">Pokud je předán parametr -o, výsledek dotazu je uchován v %% lokální kontext Python jako Pandas dataframe.</span><span class="sxs-lookup"><span data-stu-id="4b579-155">If the -o parameter is passed, the result of the query is persisted in the %%local Python context as a Pandas dataframe.</span></span>

<span data-ttu-id="4b579-156">Pro další informace o jádrech pro poznámkové bloky Jupyter a předdefinovanou "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="4b579-156">For more information on the kernels for Jupyter notebooks and the predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a><span data-ttu-id="4b579-157">Načítání dat a vytvořte rámeček vyčištěnými dat</span><span class="sxs-lookup"><span data-stu-id="4b579-157">Ingest data and create a cleaned data frame</span></span>
<span data-ttu-id="4b579-158">Tato část obsahuje kód pro řadu úkoly vyžadované ke zpracování příjmu dat do má vypočítat skóre.</span><span class="sxs-lookup"><span data-stu-id="4b579-158">This section contains the code for a series of tasks required to ingest the data to be scored.</span></span> <span data-ttu-id="4b579-159">Číst v připojené k ukázce 0,1 % taxíkem služební cestě a tarif souboru (uložený jako soubor TSV), formát data a poté vytvoří vyčištění dat rámce.</span><span class="sxs-lookup"><span data-stu-id="4b579-159">Read in a joined 0.1% sample of the taxi trip and fare file (stored as a .tsv file), format the data, and then creates a clean data frame.</span></span>

<span data-ttu-id="4b579-160">Taxíkem služební cestě a tarif soubory byly spojené na základě na postup uvedený v: [Team datové vědy procesu v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="4b579-160">The taxi trip and fare files were joined based on the procedure provided in the: [The Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) topic.</span></span>

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF THE FILE FROM HEADER
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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="4b579-161">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-161">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-162">Doba k provedení výše buňky: 46.37 sekund</span><span class="sxs-lookup"><span data-stu-id="4b579-162">Time taken to execute above cell: 46.37 seconds</span></span>

## <a name="prepare-data-for-scoring-in-spark"></a><span data-ttu-id="4b579-163">Příprava dat pro vyhodnocování v Spark</span><span class="sxs-lookup"><span data-stu-id="4b579-163">Prepare data for scoring in Spark</span></span>
<span data-ttu-id="4b579-164">V této části ukazuje, jak index, kódovat a škálování kategorií funkce, které chcete připravit je pro použití v MLlib pod dohledem learning algoritmy pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="4b579-164">This section shows how to index, encode, and scale categorical features to prepare them for use in MLlib supervised learning algorithms for classification and regression.</span></span>

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a><span data-ttu-id="4b579-165">Funkce transformace: index a kódování kategorií funkce pro vstup do modely pro vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="4b579-165">Feature transformation: index and encode categorical features for input into models for scoring</span></span>
<span data-ttu-id="4b579-166">V této části ukazuje, jak index kategorizovaná data pomocí `StringIndexer` a kódování funkcí s `OneHotEncoder` vstup na modely.</span><span class="sxs-lookup"><span data-stu-id="4b579-166">This section shows how to index categorical data using a `StringIndexer` and encode features with `OneHotEncoder` input into the models.</span></span>

<span data-ttu-id="4b579-167">[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kóduje sloupec řetězce popisků ke sloupci popisek indexy.</span><span class="sxs-lookup"><span data-stu-id="4b579-167">The [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) encodes a string column of labels to a column of label indices.</span></span> <span data-ttu-id="4b579-168">Indexy jsou seřazené podle četnosti popisek.</span><span class="sxs-lookup"><span data-stu-id="4b579-168">The indices are ordered by label frequencies.</span></span> 

<span data-ttu-id="4b579-169">[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mapuje sloupec popisek indexů ke sloupci binárního vektory, s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4b579-169">The [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) maps a column of label indices to a column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="4b579-170">Toto kódování umožňuje algoritmy, které očekávají průběžné cenná funkce, jako je logistic regression, má být použita pro kategorií funkce.</span><span class="sxs-lookup"><span data-stu-id="4b579-170">This encoding allows algorithms that expect continuous valued features, such as logistic regression, to be applied to categorical features.</span></span>

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
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="4b579-171">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-171">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-172">Doba k provedení výše buňky: 5.37 sekund</span><span class="sxs-lookup"><span data-stu-id="4b579-172">Time taken to execute above cell: 5.37 seconds</span></span>

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a><span data-ttu-id="4b579-173">Vytvoření RDD objektů s poli funkce pro vstup na modely</span><span class="sxs-lookup"><span data-stu-id="4b579-173">Create RDD objects with feature arrays for input into models</span></span>
<span data-ttu-id="4b579-174">Tato část obsahuje kód, který ukazuje, jak index kategorií textová data jako objekt RDD a jeden horkou zakódovat je, proto ji můžete použít pro trénování a testování MLlib logistic regression a na základě stromu modely.</span><span class="sxs-lookup"><span data-stu-id="4b579-174">This section contains code that shows how to index categorical text data as an RDD object and one-hot encode it so it can be used to train and test MLlib logistic regression and tree-based models.</span></span> <span data-ttu-id="4b579-175">Indexované data jsou uložena v [odolné distribuované datovou sadu (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objekty.</span><span class="sxs-lookup"><span data-stu-id="4b579-175">The indexed data is stored in [Resilient Distributed Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objects.</span></span> <span data-ttu-id="4b579-176">Jedná se o základní abstrakci v Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-176">These are the basic abstraction in Spark.</span></span> <span data-ttu-id="4b579-177">Objekt RDD představuje kolekci neměnné, oddílů elementů, které lze provozovat na paralelně s Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-177">An RDD object represents an immutable, partitioned collection of elements that can be operated on in parallel with Spark.</span></span>

<span data-ttu-id="4b579-178">Také obsahuje kód, který ukazuje, jak škálování dat pomocí `StandardScalar` poskytované MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD), oblíbených algoritmus pro trénování širokou škálu modely machine learning.</span><span class="sxs-lookup"><span data-stu-id="4b579-178">It also contains code that shows how to scale data with the `StandardScalar` provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of machine learning models.</span></span> <span data-ttu-id="4b579-179">[StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) se používá funkce, které chcete odchylku jednotky škálování.</span><span class="sxs-lookup"><span data-stu-id="4b579-179">The [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) is used to scale the features to unit variance.</span></span> <span data-ttu-id="4b579-180">Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle.</span><span class="sxs-lookup"><span data-stu-id="4b579-180">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in the objective function.</span></span> 

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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="4b579-181">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-181">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-182">Doba k provedení výše buňky: 11.72 sekund</span><span class="sxs-lookup"><span data-stu-id="4b579-182">Time taken to execute above cell: 11.72 seconds</span></span>

## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a><span data-ttu-id="4b579-183">Stanovení skóre s modelem Logistic Regression a ukládání výstupu do objektu blob</span><span class="sxs-lookup"><span data-stu-id="4b579-183">Score with the Logistic Regression Model and save output to blob</span></span>
<span data-ttu-id="4b579-184">Kód v této části ukazuje, jak načíst Logistic regresní Model, který byl uložen v úložišti objektů blob v Azure a použít ho k předvídání, zda je na cestě taxíkem placené tip, skóre s standardní klasifikace metriky a potom uložte a vykreslení výsledky do objektu blob stora ge.</span><span class="sxs-lookup"><span data-stu-id="4b579-184">The code in this section shows how to load a Logistic Regression Model that has been saved in Azure blob storage and use it to predict whether or not a tip is paid on a taxi trip, score it with standard classification metrics, and then save and plot the results to blob storage.</span></span> <span data-ttu-id="4b579-185">Scored výsledky jsou uloženy v RDD objekty.</span><span class="sxs-lookup"><span data-stu-id="4b579-185">The scored results are stored in RDD objects.</span></span> 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="4b579-186">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-186">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-187">Doba k provedení výše buňky: 19.22 sekund</span><span class="sxs-lookup"><span data-stu-id="4b579-187">Time taken to execute above cell: 19.22 seconds</span></span>

## <a name="score-a-linear-regression-model"></a><span data-ttu-id="4b579-188">Určení skóre modelu lineární regrese</span><span class="sxs-lookup"><span data-stu-id="4b579-188">Score a Linear Regression Model</span></span>
<span data-ttu-id="4b579-189">Použili jsme [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) ke cvičení model lineární regrese pomocí Stochastického přechodu klesání (SGD) pro optimalizaci k předvídání množství tip placené.</span><span class="sxs-lookup"><span data-stu-id="4b579-189">We used [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) to train a linear regression model using Stochastic Gradient Descent (SGD) for optimization to predict the amount of tip paid.</span></span> 

<span data-ttu-id="4b579-190">Kód v této části ukazuje, jak načíst Model lineární regrese z Azure blob storage, stanovení skóre pomocí škálovat proměnné a poté uložte výsledky zpět na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="4b579-190">The code in this section shows how to load a Linear Regression Model from Azure blob storage, score using scaled variables, and then save the results back to the blob.</span></span>

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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="4b579-191">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-191">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-192">Doba k provedení výše buňky: 16.63 sekund</span><span class="sxs-lookup"><span data-stu-id="4b579-192">Time taken to execute above cell: 16.63 seconds</span></span>

## <a name="score-classification-and-regression-random-forest-models"></a><span data-ttu-id="4b579-193">Stanovení skóre klasifikace a regrese náhodných modely doménové struktury</span><span class="sxs-lookup"><span data-stu-id="4b579-193">Score classification and regression Random Forest Models</span></span>
<span data-ttu-id="4b579-194">Kód v této části ukazuje, jak načíst uložené klasifikace a regrese náhodných doménové struktury modely uloží do úložiště objektů blob v Azure, stanovení skóre výkonu s standardní třídění a regrese míry a potom uložte výsledky zpět do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4b579-194">The code in this section shows how to load the saved classification and regression Random Forest Models saved in Azure blob storage, score their performance with standard classifier and regression measures, and then save the results back to blob storage.</span></span>

<span data-ttu-id="4b579-195">[Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="4b579-195">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="4b579-196">Že kombinují mnoho rozhodovacích stromů, aby se snížilo riziko overfitting.</span><span class="sxs-lookup"><span data-stu-id="4b579-196">They combine many decision trees to reduce the risk of overfitting.</span></span> <span data-ttu-id="4b579-197">Náhodné doménových struktur dokáže zpracovat kategorií funkce rozšíření pro nastavení více třídami klasifikace, nevyžadují funkce škálování a mohli zaznamenat nelineárností a funkci interakce.</span><span class="sxs-lookup"><span data-stu-id="4b579-197">Random forests can handle categorical features, extend to the multiclass classification setting, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="4b579-198">Náhodné doménových strukturách jsou jedním z těch nejúspěšnějších strojového učení modely pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="4b579-198">Random forests are one of the most successful machine learning models for classification and regression.</span></span>

<span data-ttu-id="4b579-199">[Spark.mllib](http://spark.apache.org/mllib/) podporuje náhodných doménové struktury pro více třídami a binární klasifikaci a pro regresní pomocí funkce nepřetržitý a kategorií.</span><span class="sxs-lookup"><span data-stu-id="4b579-199">[spark.mllib](http://spark.apache.org/mllib/) supports random forests for binary and multiclass classification and for regression, using both continuous and categorical features.</span></span> 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="4b579-200">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-200">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-201">Doba k provedení výše buňky: 31.07 sekund</span><span class="sxs-lookup"><span data-stu-id="4b579-201">Time taken to execute above cell: 31.07 seconds</span></span>

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a><span data-ttu-id="4b579-202">Stanovení skóre klasifikace a regrese přechodu zvyšovat skóre stromu modely</span><span class="sxs-lookup"><span data-stu-id="4b579-202">Score classification and regression Gradient Boosting Tree Models</span></span>
<span data-ttu-id="4b579-203">Kód v této části ukazuje, jak načíst klasifikace a regrese přechodu zvyšovat skóre stromu modely z Azure blob storage, stanovení skóre výkonu s standardní třídění a regrese opatření a poté uložte výsledky zpět do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4b579-203">The code in this section shows how to load classification and regression Gradient Boosting Tree Models from Azure blob storage, score their performance with standard classifier and regression measures, and then save the results back to blob storage.</span></span> 

<span data-ttu-id="4b579-204">**Spark.mllib** podporuje GBTs pro binární klasifikaci a pro regresní pomocí funkce nepřetržitý a kategorií.</span><span class="sxs-lookup"><span data-stu-id="4b579-204">**spark.mllib** supports GBTs for binary classification and for regression, using both continuous and categorical features.</span></span> 

<span data-ttu-id="4b579-205">[Přechodu zvyšovat skóre stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="4b579-205">[Gradient Boosting Trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="4b579-206">GBTs cvičení stromů rozhodnutí interaktivně, aby se minimalizoval funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="4b579-206">GBTs train decision trees iteratively to minimize a loss function.</span></span> <span data-ttu-id="4b579-207">GBTs může zpracovat kategorií funkce, nevyžadují funkce škálování a mohli zaznamenat nelineárností a funkci interakce.</span><span class="sxs-lookup"><span data-stu-id="4b579-207">GBTs can handle categorical features, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="4b579-208">Můžete také používají v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="4b579-208">They can also be used in a multiclass-classification setting.</span></span>

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="4b579-209">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-209">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-210">Doba k provedení nad buňku: 14.6 sekund</span><span class="sxs-lookup"><span data-stu-id="4b579-210">Time taken to execute above cell: 14.6 seconds</span></span>

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a><span data-ttu-id="4b579-211">Vyčištění objektů z paměti a tisk skóre pro magnitudu umístění souborů</span><span class="sxs-lookup"><span data-stu-id="4b579-211">Clean up objects from memory and print scored file locations</span></span>
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


<span data-ttu-id="4b579-212">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="4b579-212">**OUTPUT:**</span></span>

<span data-ttu-id="4b579-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span><span class="sxs-lookup"><span data-stu-id="4b579-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span></span>

<span data-ttu-id="4b579-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span><span class="sxs-lookup"><span data-stu-id="4b579-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span></span>

<span data-ttu-id="4b579-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span><span class="sxs-lookup"><span data-stu-id="4b579-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span></span>

<span data-ttu-id="4b579-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span><span class="sxs-lookup"><span data-stu-id="4b579-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span></span>

<span data-ttu-id="4b579-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span><span class="sxs-lookup"><span data-stu-id="4b579-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span></span>

<span data-ttu-id="4b579-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span><span class="sxs-lookup"><span data-stu-id="4b579-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span></span>

## <a name="consume-spark-models-through-a-web-interface"></a><span data-ttu-id="4b579-219">Využívat modely Spark pomocí webového rozhraní</span><span class="sxs-lookup"><span data-stu-id="4b579-219">Consume Spark Models through a web interface</span></span>
<span data-ttu-id="4b579-220">Spark poskytuje mechanismus vzdáleně odeslat úlohy batch nebo interaktivní dotazy pomocí rozhraní REST s komponenty s názvem Livy.</span><span class="sxs-lookup"><span data-stu-id="4b579-220">Spark provides a mechanism to remotely submit batch jobs or interactive queries through a REST interface with a component called Livy.</span></span> <span data-ttu-id="4b579-221">Livy je povoleno ve výchozím nastavení v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-221">Livy is enabled by default on your HDInsight Spark cluster.</span></span> <span data-ttu-id="4b579-222">Další informace o Livy najdete v tématu: [úlohy odeslání Spark vzdáleně pomocí Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="4b579-222">For more information on Livy, see: [Submit Spark jobs remotely using Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span></span> 

<span data-ttu-id="4b579-223">Livy můžete vzdáleně odeslat úlohu, která dávky skóre soubor, který je uložený v objektu blob Azure a pak zapíše výsledky do jiného objektu blob.</span><span class="sxs-lookup"><span data-stu-id="4b579-223">You can use Livy to remotely submit a job that batch scores a file that is stored in an Azure blob and then writes the results to another blob.</span></span> <span data-ttu-id="4b579-224">K tomuto účelu můžete odeslat skript Python z</span><span class="sxs-lookup"><span data-stu-id="4b579-224">To do this, you upload the Python script from</span></span>  
<span data-ttu-id="4b579-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) do objektu blob clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) to the blob of the Spark cluster.</span></span> <span data-ttu-id="4b579-226">Můžete použít nástroje, jako je **Microsoft Azure Storage Explorer** nebo **AzCopy** zkopírujte skript do objektu blob clusteru.</span><span class="sxs-lookup"><span data-stu-id="4b579-226">You can use a tool like **Microsoft Azure Storage Explorer** or **AzCopy** to copy the script to the cluster blob.</span></span> <span data-ttu-id="4b579-227">V našem případě jsme skript, který chcete nahrát ***wasb:///example/python/ConsumeGBNYCReg.py***.</span><span class="sxs-lookup"><span data-stu-id="4b579-227">In our case we uploaded the script to ***wasb:///example/python/ConsumeGBNYCReg.py***.</span></span>   

> [!NOTE]
> <span data-ttu-id="4b579-228">Přístupové klávesy, které budete potřebovat naleznete na portálu pro účet úložiště související s clusterem Spark.</span><span class="sxs-lookup"><span data-stu-id="4b579-228">The access keys that you need can be found on the portal for the storage account associated with the Spark cluster.</span></span> 
> 
> 

<span data-ttu-id="4b579-229">Po nahrání do tohoto umístění tento skript se spouští v rámci clusteru Spark v distribuované kontextu.</span><span class="sxs-lookup"><span data-stu-id="4b579-229">Once uploaded to this location, this script runs within the Spark cluster in a distributed context.</span></span> <span data-ttu-id="4b579-230">Načítá modelu a spouští na vstupní soubory na základě modelu předpovědi.</span><span class="sxs-lookup"><span data-stu-id="4b579-230">It loads the model and runs predictions on input files based on the model.</span></span>  

<span data-ttu-id="4b579-231">Tento skript můžete spustit vzdáleně pomocí jednoduchého požadavku HTTPS nebo REST na Livy.</span><span class="sxs-lookup"><span data-stu-id="4b579-231">You can invoke this script remotely by making a simple HTTPS/REST request on Livy.</span></span>  <span data-ttu-id="4b579-232">Zde je příkaz curl k vytvoření žádosti HTTP určený k vyvolání skript Pythonu vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="4b579-232">Here is a curl command to construct the HTTP request to invoke the Python script remotely.</span></span> <span data-ttu-id="4b579-233">Nahraďte příslušnými hodnotami pro váš cluster Spark CLUSTERLOGIN, CLUSTERPASSWORD, název clusteru.</span><span class="sxs-lookup"><span data-stu-id="4b579-233">Replace CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME with the appropriate values for your Spark cluster.</span></span>

    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

<span data-ttu-id="4b579-234">Jakýkoli jazyk ve vzdáleném systému slouží k vyvolání úlohy Spark pomocí Livy tím, že jednoduché volání HTTPS se základním ověřováním.</span><span class="sxs-lookup"><span data-stu-id="4b579-234">You can use any language on the remote system to invoke the Spark job through Livy by making a simple HTTPS call with Basic Authentication.</span></span>   

> [!NOTE]
> <span data-ttu-id="4b579-235">Je vhodné na používání knihovny, Python požadavky při provádění této volání protokolu HTTP, ale není momentálně nainstalována ve výchozím nastavení v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="4b579-235">It would be convenient to use the Python Requests library when making this HTTP call, but it is not currently installed by default in Azure Functions.</span></span> <span data-ttu-id="4b579-236">Aby se místo toho používat starší knihovny HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b579-236">So older HTTP libraries are used instead.</span></span>   
> 
> 

<span data-ttu-id="4b579-237">Tady je kód Python pro volání protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="4b579-237">Here is the Python code for the HTTP call:</span></span>

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


<span data-ttu-id="4b579-238">Můžete také přidat tento kód Python, aby [Azure Functions](https://azure.microsoft.com/documentation/services/functions/) k aktivaci úlohy odeslání Spark, která skóre na základě různých událostí jako časovače, vytvoření nebo aktualizace objektu blob objektu blob.</span><span class="sxs-lookup"><span data-stu-id="4b579-238">You can also add this Python code to [Azure Functions](https://azure.microsoft.com/documentation/services/functions/) to trigger a Spark job submission that scores a blob based on various events like a timer, creation, or update of a blob.</span></span> 

<span data-ttu-id="4b579-239">Pokud dáváte přednost volné klientského prostředí kódu, použijte [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) k vyvolání Spark dávkového vyhodnocování definováním akce HTTP na **logiku aplikace Návrhář** a nastavení jeho parametry.</span><span class="sxs-lookup"><span data-stu-id="4b579-239">If you prefer a code free client experience, use the [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) to invoke the Spark batch scoring by defining an HTTP action on the **Logic Apps Designer** and setting its parameters.</span></span> 

* <span data-ttu-id="4b579-240">Z portálu Azure vytvořit novou aplikaci logiky výběrem **+ nový** -> **Web + mobilní** -> **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="4b579-240">From Azure portal, create a new Logic App by selecting **+New** -> **Web + Mobile** -> **Logic App**.</span></span> 
* <span data-ttu-id="4b579-241">Se zprovoznit **logiku aplikace Návrhář**, zadejte název aplikace logiky a plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="4b579-241">To bring up the **Logic Apps Designer**, enter the name of the Logic App and App Service Plan.</span></span>
* <span data-ttu-id="4b579-242">Vyberte akci HTTP a zadejte parametry vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="4b579-242">Select an HTTP action and enter the parameters shown in the following figure:</span></span>

![Návrhář aplikace logiky](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a><span data-ttu-id="4b579-244">Co dále?</span><span class="sxs-lookup"><span data-stu-id="4b579-244">What's next?</span></span>
<span data-ttu-id="4b579-245">**Křížové ověření a hyperparameter (vymetání) komínů**: najdete v části [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí sweeping křížové ověření a technologie hyper parametr.</span><span class="sxs-lookup"><span data-stu-id="4b579-245">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping.</span></span>

