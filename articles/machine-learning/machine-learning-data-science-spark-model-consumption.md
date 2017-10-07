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
# <a name="operationalize-spark-built-machine-learning-models"></a>Zprovoznit learning modely vytvořené Spark počítače
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Toto téma ukazuje, jak toooperationalize modelu uloženého strojového učení (ML) používá Python na HDInsight Spark clusterů. Popisuje, jak tooload počítač learning modely, které byly vytvořené pomocí Spark MLlib a uložené v Azure Blob Storage (WASB) a jak tooscore je s datové sady, které byly uloženy také v WASB. Zobrazuje jak toopre proces hello vstupní data, pomocí funkce transformace hello indexování a kódování funkce v sadě nástrojů MLlib hello a jak toocreate s popiskem bodu datový objekt, který lze použít jako vstup pro vyhodnocování s modely hello ML. Hello modely použité pro vyhodnocování zahrnují lineární regrese, Logistic Regression, náhodné modely doménové struktury a modely stromu přechodu zvyšovat skóre.

## <a name="spark-clusters-and-jupyter-notebooks"></a>Clustery Spark a poznámkové bloky Jupyter
Postup instalace a toooperationalize kód hello ML model jsou uvedené v tomto názorném postupu pro používání clusteru služby HDInsight Spark 1.6, jakož i cluster Spark 2.0. Hello kód pro tyto postupy jsou také uvedené v poznámkové bloky Jupyter.

### <a name="notebook-for-spark-16"></a>Poznámkový blok pro Spark 1.6
Hello [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Poznámkový blok Jupyter ukazuje, jak toooperationalize uložené model používá Python v HDInsight clustery. 

### <a name="notebook-for-spark-20"></a>Poznámkový blok pro Spark 2.0
hello toomodify Poznámkový blok Jupyter pro toouse Spark 1.6 pomocí clusteru služby HDInsight Spark 2.0 nahradit soubor kód Python hello s [tento soubor](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py). Tento kód ukazuje, jak tooconsume hello modely vytvořené v Spark 2.0.


## <a name="prerequisites"></a>Požadavky

1. Budete potřebovat účet Azure a Spark 1.6 (nebo Spark 2.0) cluster HDInsight toocomplete tohoto návodu. V tématu hello [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny toosatisfy tyto požadavky. Toto téma obsahuje také popis hello NYC 2013 taxíkem dat použít se zde a pokyny, jak tooexecute kód z poznámkového bloku Jupyter v clusteru Spark hello. 
2. Musíte taky vytvořit hello modely machine learning toobe skóre pro magnitudu zde projdete hello [zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) téma je určené pro cluster hello Spark 1.6 nebo poznámkových bloků hello Spark 2.0. 
3. poznámkových bloků Hello Spark 2.0 používat další datové sady pro hello klasifikace úkol hello dobře známé letecká společnost na čas odeslání datové sady z 2011 a 2012. Popis toothem hello poznámkových bloků a odkazy jsou uvedeny v hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) hello Githubu úložiště, které je obsahují. Kromě toho hello kód sem a v poznámkových bloků hello propojené je obecný a by měla fungovat v jakémkoliv clusteru Spark. Pokud nepoužíváte HDInsight Spark, může být kroky instalace a správy clusteru hello mírně lišit od co se zobrazí tady. 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a>Instalační program: umístění úložiště, knihovny a hello přednastavení Spark kontextu
Spark je možné tooan tooread a zápisu objektů Blob Azure Storage (WASB). Takže existující data v ní uloženy může zpracovat pomocí Spark a hello výsledky uložené v znovu WASB.

modely toosave či soubory v WASB, musí cesta hello toobe zadán správně. Hello clusteru Spark toohello výchozí kontejner připojené můžete odkazovat pomocí cesty počínaje: *"wasb / /"*. Hello následující ukázka kódu určuje hello umístění toobe hello dat pro čtení a uložena hello cesta pro model výstup hello directory toowhich hello modelu úložiště. 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Nastavení cesty adresáře pro umístění úložiště v WASB
Modely se ukládají do: "wasb: / / / uživatel/remoteuser/NYCTaxi/modely". Pokud tato cesta není správně nastavena, modely nenačtou pro vyhodnocování.

Hello scored výsledky byly uloženy v: "wasb: / / / uživatel/remoteuser/NYCTaxi/ScoredResults". Pokud cesta toofolder hello není v pořádku, výsledky nejsou uloženy v této složce.   

> [!NOTE]
> umístění souboru cest Hello lze kopírovat a vložit do zástupné symboly hello tento kód hello výstup hello poslední buňku hello **machine-learning-data-science-spark-data-exploration-modeling.ipynb** poznámkového bloku.   
> 
> 

Tady je cesty adresářů tooset hello kódu: 

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

**VÝSTUP:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)

### <a name="import-libraries"></a>Importovat knihovny
Nastavit kontext spark a importovat potřebné knihovny s hello následující kód

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


### <a name="preset-spark-context-and-pyspark-magics"></a>Předvolby kontextu Spark a Magic PySpark
Hello jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu. Proto není nutné tooset kontexty Spark nebo Hive hello explicitně před zahájením práce s hello aplikací, které vyvíjíte. Tyto jsou dostupné ve výchozím nastavení. Tyto kontexty jsou:

* sc - pro Spark 
* sqlContext - pro Hive

Hello jádra PySpark poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%. Existují dva takové příkazy, které se používají v tyto ukázky kódu.

* **%% místní** zadat, že je kód hello v dalších řádcích spuštěn místně. Kód musí být platný kód Python.
* **%% sql -o<variable name>** 
* Provede dotaz Hive proti hello sqlContext. Pokud není předán parametr -o hello hello výsledek dotazu hello je uchován v hello %% lokální kontext Python jako Pandas dataframe.

Pro další informace o hello jádra pro poznámkové bloky Jupyter a hello předdefinované "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Načítání dat a vytvořte rámeček vyčištěnými dat
Tato část obsahuje hello kód pro řadu úloh vyžaduje tooingest hello data toobe vypočítat skóre. Čtení v připojené k ukázce 0,1 % hello taxíkem služební cestě a tarif souboru (uložený jako soubor TSV), formát hello dat a poté vytvoří vyčištění dat rámce.

Hello taxíkem služební cestě a tarif soubory byly spojené na základě na hello postup uvedený v: [hello proces vědecké účely Team dat v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md) tématu.

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

**VÝSTUP:**

Doba trvání tooexecute nad buňku: 46.37 sekund

## <a name="prepare-data-for-scoring-in-spark"></a>Příprava dat pro vyhodnocování v Spark
Tato část popisuje, jak tooindex, kódovat a škálování kategorií funkce tooprepare je pro použití v algoritmů učení MLlib pod dohledem pro klasifikaci a regrese.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Funkce transformace: index a kódování kategorií funkce pro vstup do modely pro vyhodnocování
Tato část uvádí, jak tooindex kategorizovaná data pomocí `StringIndexer` a kódování funkcí s `OneHotEncoder` vstup na modely hello.

Hello [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kóduje sloupec řetězce popisky tooa sloupce popisek indexů. Hello indexy, které jsou seřazené podle četnosti popisek. 

Hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mapuje sloupec popisek indexy tooa sloupce binární vektorů s maximálně jednu jeden – hodnotu. Toto kódování umožňuje algoritmy, které očekávají průběžné cenná funkce, jako je logistic regression funkce toocategorical toobe použít.

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

**VÝSTUP:**

Doba trvání tooexecute nad buňku: 5.37 sekund

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Vytvoření RDD objektů s poli funkce pro vstup na modely
Tato část obsahuje kód, který popisuje, jak tooindex kategorií textová data jako RDD objektů a jeden horkou zakódovat je tedy možné ji použít tootrain a testování MLlib logistic regression a na základě stromu modely. Hello indexované data jsou uložena v [odolné distribuované datovou sadu (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objekty. Jedná se o základní abstrakce hello v Spark. Objekt RDD představuje kolekci neměnné, oddílů elementů, které lze provozovat na paralelně s Spark.

Také obsahuje kód, který ukazuje, jak tooscale dat pomocí hello `StandardScalar` poskytované MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD), oblíbených algoritmus pro trénování širokou škálu modely machine learning. Hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) je použité tooscale hello funkce toounit odchylky. Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle hello. 

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

**VÝSTUP:**

Doba trvání tooexecute nad buňku: 11.72 sekund

## <a name="score-with-hello-logistic-regression-model-and-save-output-tooblob"></a>Stanovení skóre s hello Logistic regresní Model a uložte tooblob výstup
Hello kód v této části popisuje, jak úložiště objektů blob tooload Logistic regresní Model, který byl uložen v Azure a použít na cestě taxíkem toopredict, jestli je tip placené, skóre s standardní klasifikace metriky a potom uložte a vykreslení tooblob výsledky hello úložiště. Hello skóre pro magnitudu výsledky jsou uloženy v RDD objekty. 

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

**VÝSTUP:**

Doba trvání tooexecute nad buňku: 19.22 sekund

## <a name="score-a-linear-regression-model"></a>Určení skóre modelu lineární regrese
Použili jsme [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain placené model lineární regrese pomocí Stochastického přechodu klesání (SGD) pro optimalizaci toopredict hello množství tip. 

Hello kód v této části ukazuje, jak tooload Model lineární regrese z Azure blob storage, stanovení skóre pomocí škálovat proměnných a potom uložte hello výsledky zpět toohello objektů blob.

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


**VÝSTUP:**

Doba trvání tooexecute nad buňku: 16.63 sekund

## <a name="score-classification-and-regression-random-forest-models"></a>Stanovení skóre klasifikace a regrese náhodných modely doménové struktury
Hello kód v této části ukazuje, jak uložit tooload hello klasifikace a regrese náhodných doménové struktury modely uloží do úložiště objektů blob v Azure, stanovení skóre výkonu s standardní třídění a regrese míry a potom uložte hello výsledky zpět tooblob úložiště.

[Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.  Že kombinují mnoho rozhodovací stromy tooreduce hello riziko overfitting. Náhodné doménových struktur dokáže zpracovat kategorií funkce rozšířit nastavení toohello více třídami klasifikace, nevyžadují funkce škálování a jsou možné toocapture nelineárností a funkce, interakce. Náhodné doménových strukturách jsou jedním z hello těch nejúspěšnějších modelů strojového učení pro klasifikaci a regrese.

[Spark.mllib](http://spark.apache.org/mllib/) podporuje náhodných doménové struktury pro více třídami a binární klasifikaci a pro regresní pomocí funkce nepřetržitý a kategorií. 

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

**VÝSTUP:**

Doba trvání tooexecute nad buňku: 31.07 sekund

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Stanovení skóre klasifikace a regrese přechodu zvyšovat skóre stromu modely
Hello kód v této části ukazuje, jak tooload klasifikace a regrese přechodu zvyšovat skóre stromu modely z Azure blob storage, stanovení skóre výkonu s standardní třídění a regrese míry a potom uložte hello výsledky zpět tooblob úložiště. 

**Spark.mllib** podporuje GBTs pro binární klasifikaci a pro regresní pomocí funkce nepřetržitý a kategorií. 

[Přechodu zvyšovat skóre stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy. GBTs train rozhodovací stromy interaktivně toominimize funkci ztrátu. GBTs může zpracovat kategorií funkce, nevyžadují funkce škálování a jsou možné toocapture nelineárností a funkce, interakce. Můžete také používají v nastavení multiclass klasifikace.

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

**VÝSTUP:**

Doba trvání tooexecute nad buňku: 14.6 sekund

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Vyčištění objektů z paměti a tisk skóre pro magnitudu umístění souborů
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


**VÝSTUP:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt

## <a name="consume-spark-models-through-a-web-interface"></a>Využívat modely Spark pomocí webového rozhraní
Spark poskytuje mechanismus tooremotely odeslání dávkových úloh nebo interaktivních dotazů prostřednictvím REST rozhraní s komponenty s názvem Livy. Livy je povoleno ve výchozím nastavení v clusteru HDInsight Spark. Další informace o Livy najdete v tématu: [úlohy odeslání Spark vzdáleně pomocí Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Můžete použít Livy tooremotely odeslat úlohu, která dávky skóre soubor, který je uložený v objektu blob Azure a pak zapíše hello výsledky tooanother objektů blob. toodo, můžete nahrávat na server hello skript v jazyce Python z  
[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob clusteru Spark hello. Můžete použít nástroje, jako je **Microsoft Azure Storage Explorer** nebo **AzCopy** toocopy hello skriptu toohello clusteru blob. V našem případě jsme příliš nahrán hello skriptu***wasb:///example/python/ConsumeGBNYCReg.py***.   

> [!NOTE]
> Hello přístupové klíče, které můžete potřebovat naleznete na portálu hello pro účet úložiště hello spojené s clusterem Spark hello. 
> 
> 

Po nahrání toothis umístění tento skript se spouští v rámci clusteru Spark hello v distribuované kontextu. Načítá hello modelu a spouští předpovědi na vstupní soubory na základě modelu hello.  

Tento skript můžete spustit vzdáleně pomocí jednoduchého požadavku HTTPS nebo REST na Livy.  Zde je vzdáleně curl příkaz tooconstruct hello HTTP žádost tooinvoke hello skript v jazyce Python. Nahraďte CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME hello odpovídající hodnoty pro váš cluster Spark.

    # CURL COMMAND tooINVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Můžete použít jakýkoli jazyk u hello vzdálený systém tooinvoke hello Spark úlohy pomocí Livy tím, že jednoduché volání HTTPS se základním ověřováním.   

> [!NOTE]
> Je vhodné toouse hello Python požadavky knihovny při provádění této volání protokolu HTTP, ale není momentálně nainstalována ve výchozím nastavení v Azure Functions. Aby se místo toho používat starší knihovny HTTP.   
> 
> 

Zde je kód Python hello hello HTTP volání:

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


Můžete také přidat tento kód Python příliš[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger odeslání úlohy Spark, která skóre objekt blob podle různých událostí jako časovače, vytvoření nebo aktualizace objektu blob. 

Pokud dáváte přednost volné klientského prostředí kódu, použijte hello [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark dávkového vyhodnocování definováním akce HTTP na hello **logiku aplikace Návrhář** a nastavení jeho parametry. 

* Z portálu Azure vytvořit novou aplikaci logiky výběrem **+ nový** -> **Web + mobilní** -> **aplikace logiky**. 
* toobring až hello **logiku aplikace Návrhář**, zadejte název hello hello aplikace logiky a plán služby App Service.
* Vyberte akci HTTP a zadejte parametry hello znázorňuje následující obrázek hello:

![Návrhář aplikace logiky](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a>Co dále?
**Křížové ověření a hyperparameter (vymetání) komínů**: najdete v části [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí sweeping křížové ověření a technologie hyper parametr.

