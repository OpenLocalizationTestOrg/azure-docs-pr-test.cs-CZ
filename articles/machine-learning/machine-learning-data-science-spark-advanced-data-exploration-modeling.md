---
title: "zkoumání dat aaaAdvanced a modelování s Spark | Microsoft Docs"
description: "Použijte zkoumání dat toodo HDInsight Spark a cvičení binární klasifikace a regrese modelů pomocí křížové ověření a hyperparameter optimalizace."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f90d9a80-4eaf-437b-a914-23514390cd60
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 055c342857fd732633cec9810de69cee61db973d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Pokročilé zkoumání a modelování dat pomocí Spark
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tento návod používá HDInsight Spark toodo zkoumání a train binární klasifikace dat a modely regrese pomocí křížové ověření a hyperparameter optimalizace na základě vzorku hello NYC taxi cestě a jízdenky 2013 datovou sadu. Ho vás provede kroky hello hello [proces vědecké účely dat](http://aka.ms/datascienceprocess), klient server, pomocí HDInsight Spark clusteru pro zpracování a toostore hello dat a hello modely objektů Azure BLOB. proces Hello jsou zde popsány vizualizuje dat získaných z objektu Blob úložiště Azure a pak připraví hello data toobuild prediktivní modely. Python byl použité toocode hello řešení a tooshow hello relevantní pozemků. Tyto modely jsou sestavení pomocí hello Spark MLlib toolkit toodo binární klasifikace a regresní modelování úlohy. 

* Hello **binární klasifikace** úloha je toopredict, zda je pro cestu hello placené tip. 
* Hello **regrese** úloh je toopredict hello množství hello tip podle dalších funkcí tip. 

Hello modelování kroky také obsahovat kódu, který ukazuje, jak tootrain, hodnocení a uložit každý typ modelu. Hello téma popisuje některé z hello stejné pozadí jako hello [zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) tématu. Ale ho je "složitější," také používá křížového ověřování s hyperparameter komínů tootrain optimálně přesné klasifikace a regrese modelů. 

**Křížové ověření (odchylka nákladů)** je technika, který vyhodnocuje, jak dobře model trénink na známé sadu dat umožňuje zobecnit toopredicting hello funkce datových sad, na kterém je nebyla cvičena.  Běžná implementace použít zde je toodivide datové sady do tisíc složení a pak trénování modelu hello v kruhového dotazování na všechny kromě jednoho hello složení. se hodnotí schopnost Hello hello modelu tooprediction přesně, při testování proti hello nezávislé datové sady v tomto složení nepoužívá tootrain hello modelu.

**Optimalizace Hyperparameter** potížím hello vybrat sadu hyperparameters pro algoritmu učení, obvykle s cílem hello optimalizace měření výkonu hello algoritmus na nezávislé datové sady. **Hyperparameters** jsou hodnoty, které musí být zadán mimo hello modelu školení postupu. Předpoklady o tyto hodnoty může ovlivnit hello flexibilitu a přesnost hello modelů. Rozhodovací stromy mít hyperparameters, například jako hello potřeby hloubkou a počet nechá ve stromu hello. Podpora vektoru počítače (SVMs) vyžadují, nastavení termín snížení chybnou klasifikaci. 

Běžné způsob tooperform hyperparameter optimalizace použít zde je hledání mřížky nebo **oblouku parametr**. Tento postup se skládá provádět důkladné prohledání prostřednictvím hello hodnoty podmnožinu hello hyperparameter místo zadané pro algoritmus učení. Křížového ověření může poskytovat metriky toosort out hello optimální výsledky vyprodukované hello mřížky vyhledávacího algoritmu výkonu. Odchylka nákladů použít s obezřetností pomáhá hyperparameter limit problémy jako overfitting tootraining datový model, aby hello modelu uchovává hello kapacity tooapply toohello obecné sady dat, ze které hello jste extrahovali Cvičná data.

Hello modely, které používáme zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromů:

* [Lineární regrese s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je model lineární regrese, který používá metodu Stochastického přechodu klesání (SGD) a škálování toopredict hello tip objemy placené pro optimalizaci a funkce. 
* [Logistic regression s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) nebo "logit" regression je regresní model, který lze použít při hello závislé proměnné je klasifikace dat kategorií toodo. LBFGS je algoritmus optimalizace jako Newton, blíží algoritmus hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomocí omezené množství paměti počítače a který se často používá v machine learning.
* [Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.  Že kombinují mnoho rozhodovací stromy tooreduce hello riziko overfitting. Náhodné doménové struktury se používají pro regresní a klasifikace a dokáže zpracovat kategorií funkce a je možné rozšířit toohello více třídami klasifikace nastavení. Funkce škálování a jsou možné toocapture nelineárností a funkce interakce se nevyžadují. Náhodné doménových strukturách jsou jedním z hello těch nejúspěšnějších modelů strojového učení pro klasifikaci a regrese.
* [Přechodu boosted stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy. GBTs train rozhodovací stromy interaktivně toominimize funkci ztrátu. GBTs se používají pro regresní a klasifikace a dokáže zpracovat kategorií funkce, nevyžadují funkce škálování a mít toocapture nelineárností a funkce, interakce. Můžete také používají v nastavení multiclass klasifikace.

Modelování příklady použití odchylka nákladů a Hyperparameter oblouku se zobrazují pro problém binární klasifikace hello. Jednodušší příklady (bez parametru změny) jsou uvedeny na hello hlavní téma pro regresní úlohy. Ale v hello příloha, jsou také uvedeny ověření pomocí elastické net pro lineární regrese a odchylka nákladů pomocí parametru oblouku pro regresní náhodných doménové struktury. Hello **Elastická net** je metoda Vyřešeno regrese pro hodí lineární regrese modelů, Lineárně kombinuje hello L1 a L2 metriky jako postihy hello [laso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) a [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) metody.   

> [!NOTE]
> Sice hello Spark MLlib toolkit navrženou toowork v rozsáhlých datových sad, relativně malé ukázkové (pomocí 170 tisíc řádků, přibližně 0,1 % hello původní datové sady NYC ~ 30 Mb) se zde používá ke zvýšení pohodlí. cvičení Hello zadané tady běží efektivně (v přibližně 10 minut) v clusteru HDInsight s 2 uzlů pracovního procesu. Hello stejný kód s méně závažné změny lze použít tooprocess větší-sady dat, se změny, které pro ukládání do mezipaměti data v paměti a změna velikosti clusteru hello.
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a>Instalační program: Clustery Spark a poznámkových bloků
Kroky instalace a kódu jsou uvedené v tomto názorném postupu pro používání HDInsight Spark 1.6. Ale poznámkové bloky Jupyter jsou k dispozici pro clustery HDInsight Spark 1.6 a Spark 2.0. Popis toothem hello poznámkových bloků a odkazy jsou uvedeny v hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) hello Githubu úložiště, které je obsahují. Kromě toho hello kód sem a v poznámkových bloků hello propojené je obecný a by měla fungovat v jakémkoliv clusteru Spark. Pokud nepoužíváte HDInsight Spark, může být kroky instalace a správy clusteru hello mírně lišit od co se zobrazí tady. Pro usnadnění práce uvádíme hello odkazy toohello Jupyter notebooks Spark 1.6 a 2.0 toobe spustit v jádra pyspark hello Dobrý den Poznámkový blok Jupyter serveru:

### <a name="spark-16-notebooks"></a>Spark 1.6 poznámkových bloků

[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): obsahuje témata v poznámkovém bloku #1 a modelu vývoj pomocí hyperparameter ladění a křížové ověření.

### <a name="spark-20-notebooks"></a>Spark 2.0 poznámkových bloků

[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Tento soubor obsahuje informace o tom, jak tooperform zkoumání dat, modelování a vyhodnocování v rámci Spark 2.0 clusterů.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a>Instalační program: umístění úložiště, knihovny a hello přednastavení Spark kontextu
Spark je možné tooAzure tooread a zápisu objektu Blob Storage (WASB). Takže existující data v ní uloženy může zpracovat pomocí Spark a hello výsledky uložené v znovu WASB.

modely toosave či soubory v WASB, musí cesta hello toobe zadán správně. Hello clusteru Spark toohello výchozí kontejner připojené můžete odkazovat pomocí cesty počínaje: "wasb: / / /". Odkazují jiné umístění "wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Nastavení cesty adresáře pro umístění úložiště v WASB
Hello následující ukázka kódu určuje hello umístění toobe hello data přečíst a uložena hello cesta pro model výstup hello directory toowhich hello modelu úložiště:

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**VÝSTUP**

DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)

### <a name="import-libraries"></a>Importovat knihovny
Importujte knihovny potřebné s hello následující kód:

    # LOAD PYSPARK LIBRARIES
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
Hello jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu. Proto není nutné tooset kontexty Spark nebo Hive hello explicitně před zahájením práce s hello aplikací, které vyvíjíte. Tyto kontexty jsou dostupné ve výchozím nastavení. Tyto kontexty jsou:

* sc - pro Spark 
* sqlContext - pro Hive

Hello jádra PySpark poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%. Existují dva takové příkazy, které se používají v tyto ukázky kódu.

* **%% místní** Určuje, že je kód hello v dalších řádcích toobe provést místně. Kód musí být platný kód Python.
* **%% sql -o <variable name>**  provede dotaz Hive proti hello sqlContext. Pokud není předán parametr -o hello hello výsledek dotazu hello je uchován v hello %% lokální kontext Python jako Pandas DataFrame.

Pro další informace o hello jádra pro poznámkové bloky Jupyter a hello předdefinované "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="data-ingestion-from-public-blob"></a>Přijímání dat z veřejného objektu blob:
Hello prvním krokem v procesu vědecké účely dat hello je tooingest hello data toobe analyzovali ze zdrojů, na němž je umístěna do prostředí pro zkoumání a modelování aplikace data. Toto prostředí je Spark v tomto návodu. Tato část obsahuje hello kód toocomplete řadu úloh:

* ingestování toobe ukázková data hello modelován
* Přečtěte si hello vstupní datové sady (uložený jako soubor TSV)
* Formát a vyčištění hello data
* vytvářet a ukládat do mezipaměti objektů (RDDs nebo datových rámců) v paměti
* Zaregistrujte se jako dočasné tabulky v kontextu SQL.

Tady je kód hello přijímání dat.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_train_file.first()
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

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES hello DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Doba trvání tooexecute nad buňku: 276.62 sekund

## <a name="data-exploration--visualization"></a>Zkoumání dat a vizualizaci
Jakmile hello data vstoupila v Spark, hello dalším krokem v procesu vědecké účely dat hello je toogain podrobnější vysvětlení hello dat prostřednictvím zkoumání a vizualizace. V této části jsme zkontrolujte hello taxíkem dat pomocí dotazů SQL a vykreslení hello cílových proměnných a potenciální funkce pro visual kontroly. Konkrétně jsme vykreslení hello frekvenci počty osobní v taxi služebních cest, četnost hello tip objemu a jak se typy liší podle částka platby a typu.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a>Vykreslení histogram frekvencí počet osobní v ukázce hello taxíkem cest
Tento kód a následné fragmenty pomocí SQL magic tooquery hello ukázka a místní magic tooplot hello data.

* **SQL magic (`%%sql`)** hello jádra HDInsight PySpark podporuje dotazy na snadno vložené HiveQL pro hello sqlContext. Hello (-o VARIABLE_NAME) argument potrvají výstup hello dotazu SQL hello jako Pandas DataFrame na serveru Jupyter hello. To znamená, že je k dispozici v místním režimu hello.
* Hello  **`%%local` magic** je použít kód toorun místně na serveru Jupyter hello, což je headnode hello hello clusteru HDInsight. Obvykle použijete, `%%local` magic po hello `%%sql -o` magic je použité toorun dotazu. Parametr -o Hello by zachovat hello výstup hello místně v dotazu SQL. Potom hello `%%local` magic aktivační události hello další sadu toorun fragmenty kódu místně proti hello výstup hello dotazy SQL, který obsahuje místně trvalé. výstup Hello se automaticky vizualizuje po spuštění kódu hello.

Tento dotaz načte služebních cest hello podle počtu osobní. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Tento kód vytvoří místní data snímku z výstupu dotazu hello a zobrazuje hello data. Hello `%%local` magic vytvoří místní rámce dat, `sqlResults`, který může být použit pro vykreslení s matplotlib. 

> [!NOTE]
> Tato PySpark magic se používá více než jednou. v tomto návodu. Pokud je velká hello množství dat, by měl ukázkové toocreate data rámce, který můžete začlenit do místní paměti.
> 
> 

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Zde je osobní hello kód tooplot hello cest počty

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**VÝSTUP**

![Frekvence služebních cest podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Můžete vybrat mezi několika různých typů vizualizace (tabulky, kruhový, řádku, oblasti nebo panelu) pomocí hello **typ** tlačítka nabídky v poznámkovém bloku hello. Zobrazí se zde Hello panelu vykreslení.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Vykreslení histogram tip objemy a jak se liší podle počtu a tarif objemy osobní tip velikost.
Použít data toosample dotazu SQL...

    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25


Tuto buňku kód používá hello SQL dotaz toocreate tři pozemků hello data.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline

    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()


**VÝSTUP:** 

![Tip rozdělení částky](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip velikost podle tarif velikost](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Funkce analýzy, transformaci a data přípravy pro modelování
Tato část popisuje a zajišťuje, že kód hello postupy použít tooprepare dat pro použití v ML modelování. Ukazuje, jak toodo hello následující úkoly:

* Vytvořte novou funkci tak, že dělení čas do přihrádek doba provozu
* Index a na horkou kódování kategorií funkce
* Vytváření objektů s popiskem bod pro vstup do funkce ML
* Vytvořit náhodné vzorky dílčí hello dat a rozdělit ho na trénování a testování sad
* Funkce škálování
* Objekty mezipaměti v paměti

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a>Vytvořte novou funkci tak, že dělení časy provoz do přihrádek
Tento kód ukazuje, jak toocreate novou funkci ve vytváření oddílů provoz časy do přihrádek a poté jak toocache hello výsledné datové rámce v paměti. Ukládání do mezipaměti vede doba provádění tooimproved odolné distribuované datové sady (RDDs) a datových rámců použití opakovaně. Ano jsme mezipaměti RDDs a datové rámce v několika fázích v tomto návodu.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # hello .COUNT() GOES THROUGH hello ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES hello COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**VÝSTUP**

126050

### <a name="index-and-one-hot-encode-categorical-features"></a>Index a jeden horkou kódování kategorií funkce
Tato část uvádí, jak tooindex nebo kódování kategorií funkce pro vstup do funkce modelování hello. Dobrý den, modelování a předvídání funkce MLlib požadovat, aby se funkce s kategorií vstupní data indexované nebo kódovaný předchozí toouse. 

V závislosti na modelu hello potřebovat tooindex nebo zakódovat je různými způsoby. Například Logistic a lineární regrese modely vyžadují jeden horkou kódování, kde, například funkce s tří kategorií lze rozšířit na tři sloupce funkce, s každou obsahující 0 nebo 1 v závislosti na kategorii hello pozorování. Poskytuje MLlib [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce toodo horkou jeden kódování. Tato kodér mapuje sloupec popisek indexy tooa sloupce binární vektorů s maximálně jednu jeden – hodnotu. Toto kódování umožňuje algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression funkce toocategorical toobe použít.

Tady je hello tooindex kódu a kódování kategorií funkce:

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Doba trvání tooexecute nad buňku: 3.14 sekund

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Vytváření objektů s popiskem bod pro vstup do funkce ML
Tato část obsahuje kód, který ukazuje, jak tooindex kategorií textová data jako datový bod s popiskem typ a tooencode ho. To připraví toobe používá tootrain a testování MLlib logistic regression a jinými modely klasifikace. S popiskem bodu objekty jsou odolné distribuované datové sady (RDD) formátu způsobem, který je nutný jako vstupní data pro většinu ML algoritmy v MLlib. A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.

Tady je hello tooindex kódu a kódování textu funkce pro binární klasifikaci.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Tady je kód hello tooencode a index kategorií text funkce pro analýzu lineární regrese.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a>Vytvořit náhodné vzorky dílčí hello dat a rozdělit ho na trénování a testování sad
Tento kód vytvoří náhodné vzorky dat hello (25 % tady slouží). I když to není nutné v tomto příkladu kvůli toohello velikost hello datovou sadu, ukážeme, jak můžete ukázková data hello sem. Pak víte, jak toouse pro váš vlastní problém v případě potřeby. Po velká vzorky lze ušetřit čas významné při školení modelů. Další jsme rozdělit hello ukázka na školení část (v tomto poli 75 %) a testování toouse část (v tomto poli 25 %) v klasifikaci a regresní modelování.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand

    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Doba trvání tooexecute nad buňku: 0.31 sekund

### <a name="feature-scaling"></a>Funkce škálování
Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle hello. Hello kódu pro funkce škálování používá hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello funkce toounit odchylky. Pochází od MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD). SGD je Oblíbené algoritmus pro trénování širokou škálu jiných modelů strojového učení například Vyřešeno regresí nebo support vector počítače (SVM).   

> [!TIP]
> Našli jsme hello LinearRegressionWithSGD algoritmus toobe citlivé toofeature škálování.   
> 
> 

Zde je hello kód tooscale proměnné pro použití s hello Vyřešeno lineární SGD algoritmus.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Doba trvání tooexecute nad buňku: 11.67 sekund

### <a name="cache-objects-in-memory"></a>Objekty mezipaměti v paměti
ukládání do mezipaměti hello vstupní data rámce objekty používá pro klasifikaci, regresi a škálovat funkce může snížit Hello čas potřebný pro trénování a testování ML algoritmů.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP** 

Doba trvání tooexecute nad buňku: 0,13 sekund

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Předpovědět, zda je tip placené s modely binární klasifikace
Tato část uvádí, jak použití, tři modely pro predikci hello binární klasifikace úlohy zda tip je placené taxíkem cesty. modely Hello uvedené jsou:

* Logistic regression 
* Náhodné doménové struktury
* Přechodu zvýšení skóre stromů

Každý model vytváření části kódu je rozdělená do kroků: 

1. **Model školení** dat pomocí jednu sadu parametrů
2. **Model vyhodnocení** na testovací datové sady s metriky
3. **Ukládání modelu** v objektu blob pro budoucí spotřeba

Ukážeme, jak toodo křížové ověření (odchylka nákladů) s parametrem komínů dvěma způsoby:

1. Pomocí **Obecné** vlastní kód, který může být použité tooany algoritmus MLlib a tooany parametr nastaví v algoritmu. 
2. Pomocí hello **pySpark CrossValidator kanálu funkce**. Všimněte si, že CrossValidator má několik omezení pro Spark 1.5.0: 
   
   * Modely kanálu nelze uložit nebo trvalá pro budoucí spotřeby.
   * Nelze použít pro každý parametr v modelu.
   * Nelze použít pro každý MLlib algoritmus.

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-hello-logistic-regression-algorithm-for-binary-classification"></a>Obecná křížové ověření a hyperparameter (vymetání) komínů použít s hello logistic regresní algoritmus pro binární klasifikaci
Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu. cvičení modelu Hello pomocí křížové ověření (odchylka nákladů) a hyperparameter (vymetání) komínů implementuje pomocí vlastní kód, který může být použité tooany hello učení algoritmy v MLlib.   

> [!NOTE]
> Hello provádění tohoto vlastního kódu odchylka nákladů může trvat několik minut.
> 
> 

**Cvičení hello logistic regresní model pomocí odchylka nákladů a hyperparameter (vymetání) komínů**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics

    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)

    # SET NUM FOLDS AND NUM PARAMETER SETS tooSWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric

    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)


    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Koeficienty: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Zachycení:-0.0111216486893

Doba trvání tooexecute nad buňku: 14.43 sekund

**Vyhodnocení modelu binární klasifikace hello o standardní metriky**

Hello kód v této části ukazuje, jak tooevaluate logistic regresní model proti testovací data sada, včetně vykreslení hello: křivka ROC.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Oblasti v rámci PR = 0.985336538462

Oblasti v rámci ROC = 0.983383274312

Souhrnné statistiky

Přesnost = 0.984174341679

Odvolat = 0.984174341679

F1 Stanovení skóre = 0.984174341679

Doba trvání tooexecute nad buňku: 2.67 sekund

**Vykreslení křivka ROC hello.**

Hello *predictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku hello. *tmp_results* je možné použít toodo dotazy a výstup výsledků do hello sqlResults dat rámce pro vykreslení. Tady je kód hello.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Zde je hello kód toomake předpovědi a vykreslení hello křivka ROC.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVES
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**VÝSTUP**

![Křivka ROC logistic regression pro obecný přístup](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

**Zachovat modelu do objektu BLOB pro budoucí spotřeba**

Hello kód v této části ukazuje, jak toosave hello logistic regresní model pro používání.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;

    logitBest.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";


**VÝSTUP**

Doba trvání tooexecute nad buňku: 34.57 sekund

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>Pomocí funkce kanálu CrossValidator na MLlib s modelem logistic regression (elastické regrese)
Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu. cvičení modelu Hello pomocí křížového ověření (odchylka nákladů) a hyperparameter (vymetání) komínů implementováno s hello MLlib CrossValidator kanálu funkce pro odchylka nákladů s oblouku parametr.   

> [!NOTE]
> Hello provádění tohoto kódu odchylka MLlib nákladů může trvat několik minut.
> 
> 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc

    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

**VÝSTUP**

Doba trvání tooexecute nad buňku: 107.98 sekund

**Vykreslení křivka ROC hello.**

Hello *predictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku hello. *tmp_results* je možné použít toodo dotazy a výstup výsledků do hello sqlResults dat rámce pro vykreslení. Tady je kód hello.

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Zde je kód hello tooplot: křivka ROC hello.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc

    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    #PLOT
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**VÝSTUP**

![Pomocí CrossValidator na MLlib křivka ROC logistic regression](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a>Klasifikace náhodných doménové struktury
Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit regrese náhodných doménové struktury, který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Oblasti v rámci ROC = 0.985336538462

Doba trvání tooexecute nad buňku: 26.72 sekund

### <a name="gradient-boosting-trees-classification"></a>Přechodu zvýšení skóre klasifikace stromů
Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Oblasti v rámci ROC = 0.985336538462

Doba trvání tooexecute nad buňku: 28.13 sekund

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Předpověď tip velikost s modely regrese (nepoužíváte odchylka nákladů)
V této části ukazuje, jak použít, tři modely pro úlohy regrese hello: předpovědi velikost tip hello placené taxíkem cesty na základě jiných funkcí tip. modely Hello uvedené jsou:

* Vyřešeno lineární regrese
* Náhodné doménové struktury
* Přechodu zvýšení skóre stromů

Tyto modely byly popsané v úvodu hello. Každý model vytváření části kódu je rozdělená do kroků: 

1. **Model školení** dat pomocí jednu sadu parametrů
2. **Model vyhodnocení** na testovací datové sady s metriky
3. **Ukládání modelu** v objektu blob pro budoucí spotřeba   

> AZURE Poznámka: Křížové ověření není použít s hello tři modely Regrese v této části, protože to byla vidět v souvislosti s modely logistic regression hello. Příklad znázorňující, jak je uvedený toouse odchylka nákladů s elastické Net pro lineární regrese v hello příloha tohoto tématu.
> 
> AZURE Poznámka: V našich zkušeností, může být problémy s konvergence LinearRegressionWithSGD modelů a parametry potřebovat toobe změnit/optimalizované pečlivě pro získání platný model. Škálování proměnných výrazně pomáhá s konvergence. Elastické net regrese uvedené v tématu toothis hello příloha, mohou sloužit také místo LinearRegressionWithSGD.
> 
> 

### <a name="linear-regression-with-sgd"></a>Lineární regrese s SGD
Hello kód v této části ukazuje, jak toouse škálovat funkce tootrain lineární regrese, který stochastického přechodu klesání (SGD) používá pro optimalizaci, a jak tooscore, hodnocení a uložit hello model v Azure Blob Storage (WASB).

> [!TIP]
> V našich zkušeností může být problémy s hello konvergence LinearRegressionWithSGD modelů a parametry potřebovat toobe změnit/optimalizované pečlivě pro získání platný model. Škálování proměnných výrazně pomáhá s konvergence.
> 
> 

    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES tooTRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

Koeficienty: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Zachytávat: 0.854507624459

RMSE = 1.23485131376

R sqr = 0.597963951127

Doba trvání tooexecute nad buňku: 38.62 sekund

### <a name="random-forest-regression"></a>Regrese náhodných doménové struktury
Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit model náhodných doménové struktury, který předpovídá velikost tip pro hello NYC taxíkem cestě data.   

> [!NOTE]
> Křížové ověření s parametrem komínů pomocí vlastního kódu jsou uvedené v dodatku hello.
> 
> 

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**VÝSTUP**

RMSE = 0.931981967875

R sqr = 0.733445485802

Doba trvání tooexecute nad buňku: 25.98 sekund

### <a name="gradient-boosting-trees-regression"></a>Přechodu zvýšení skóre regresní stromy
Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá velikost tip pro hello NYC taxíkem cestě data.

** Natrénování a vyhodnocení **

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

RMSE = 0.928172197114

R sqr = 0.732680354389

Doba trvání tooexecute nad buňku: 20.9 sekund

**Vykreslení.**

*tmp_results* je registrován jako tabulku Hive v předchozí buňku hello. Výsledky z tabulky hello jsou výstupem do hello *sqlResults* dat rámce pro vykreslení. Tady je kód hello

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Zde je hello kód tooplot hello dat pomocí hello Jupyter server.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np

    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Skutečný vs předpovědět tip objemy](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Dodatek: Další regresní úlohy křížového ověření pomocí parametru změny
Tento dodatek obsahuje kód znázorňující způsob toodo odchylka nákladů pomocí elastické net pro lineární regrese a jak toodo odchylka nákladů s parametrem oblouku pomocí vlastní kód pro regresní náhodných doménové struktury.

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Křížové ověření pomocí Elastická net pro lineární regrese
Hello kód v této části ukazuje, jak toodo křížové ověření pomocí Elastická net pro lineární regrese a jak tooevaluate hello model testovacích datech.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder

    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 

    # DEFINE PIPELINE 
    # SIMPLY hello MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses hello best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT tooDF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

Doba trvání tooexecute nad buňku: 161.21 sekund

**Vyhodnocení s metrikou R SQR**

*tmp_results* je registrován jako tabulku Hive v předchozí buňku hello. Výsledky z tabulky hello jsou výstupem do hello *sqlResults* dat rámce pro vykreslení. Tady je kód hello

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Zde je kód toocalculate hello R sqr.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**VÝSTUP**

R sqr = 0.619184907088

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Křížové ověření pomocí parametru oblouku pomocí vlastní kód pro regresní náhodných doménové struktury
Hello kód v této části ukazuje, jak toodo křížové ověření pomocí parametru oblouku pomocí vlastní kód pro regresní náhodných doménové struktury a jak tooevaluate hello model testovacích datech.

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid

    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))

    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # SPECIFY NUMFOLDS AND ARRAY tooHOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric

    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**VÝSTUP**

RMSE = 0.906972198262

R sqr = 0.740751197012

Doba trvání tooexecute nad buňku: 69.17 sekund

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Vyčištění objektů z paměti a umístění tiskové modelu
Použití `unpersist()` toodelete objekty uložené v mezipaměti v paměti.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


**VÝSTUP**

PythonRDD [122] v RDD v PythonRDD.scala: 43

** Výtisk cesta toomodel soubory toobe použít v poznámkovém bloku spotřeba hello. ** tooconsume a skóre nezávislou data-set, potřebujete toocopy a vložte tyto názvy souborů hello "Spotřeba poznámkového bloku".

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**VÝSTUP**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Co dále?
Teď, když jste vytvořili regrese a klasifikace modely s hello Spark MlLib, jak jste připravené toolearn tooscore a vyhodnocovat u nich těchto modelů.

**Model spotřeby:** toolearn jak tooscore a vyhodnotit hello klasifikace a regrese modelů vytvořených v tomto tématu najdete v tématu [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).

