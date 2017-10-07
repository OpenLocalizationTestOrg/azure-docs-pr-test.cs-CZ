---
title: "aaaData vědecké účely pomocí Scala a Spark v Azure | Microsoft Docs"
description: "Jak toouse Scala pro pod dohledem strojového učení úlohy s hello Spark škálovatelné MLlib a Spark ML balíčky v clusteru Azure HDInsight Spark."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a7c97153-583e-48fe-b301-365123db3780
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;deguhath
ms.openlocfilehash: e32ebd0b91417183fe48ee10ebc7929fd9605762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-using-scala-and-spark-on-azure"></a>Vědecké zkoumání dat pomocí Scala a Spark v Azure
Tento článek ukazuje, jak balíčky toouse Scala pro úkoly pod dohledem strojového učení s hello škálovatelné MLlib Spark a Spark ML v clusteru Azure HDInsight Spark. Provede vás prostřednictvím hello úloh, které tvoří hello [proces vědecké zpracování dat](http://aka.ms/datascienceprocess): přijímání dat a zkoumání, vizualizace, funkce analýzy, modelování a spotřeba modelu. modely Hello v článku hello obsahovat logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromy (GBTs), kromě tootwo běžné pod dohledem úkoly strojového učení:

* Regrese problému: předpovědi hello tip velikost ($) pro cestu taxíkem
* Binární klasifikace: předpovědi tip nebo cesty taxíkem žádné tip (1 nebo 0)

Hello modelování proces vyžaduje trénování a hodnocení na testovací datové sady a relevantní přesnost metriky. V tomto článku, můžete informace, jak toostore tyto modely v Azure Blob storage a jak tooscore a vyhodnotit jejich prediktivní výkonu. Tento článek se týká také pokročilejší témata z jak toooptimize modelů pomocí (vymetání) křížové ověření a technologie hyper parametr komínů hello. použít data Hello je ukázka hello 2013 NYC taxíkem služební cestě a tarif datových sad dostupná na Githubu.

[Scala](http://www.scala-lang.org/), jazyk založené na virtuálním počítači Java hello, integruje koncepty jazyka objektově orientované a funkční. Je škálovatelná jazyk, který je dobře hodí toodistributed zpracování v cloudu hello a běží na clustery Spark v Azure.

[Spark](http://spark.apache.org/) framework paralelní zpracování open source, který podporuje v paměti zpracovává tooboost hello výkon aplikací analýzy velkých objemů dat. modul zpracování Spark Hello je vytvořené pro rychlost, snadné použití a sofistikované analytics. Možnosti v paměti distribuované výpočtů Spark ho nastavit správnou volbu pro iterativní algoritmy v machine learning a grafů výpočty. Hello [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) balíček poskytuje jednotnou sadu vysoké úrovně rozhraní API vytvořená na základě dat snímků, které vám pomůžou vytvářet a ladit praktické strojového učení kanály. [MLlib](http://spark.apache.org/mllib/) je Spark škálovatelné machine learning knihovny, která přináší modelování možnosti toothis distribuovaném prostředí.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) je nabídka Azure hostovaná hello Spark open source. Zahrnuje taky podporu poznámkové bloky Jupyter Scala na clusteru Spark hello a můžete spustit tootransform interaktivních dotazů Spark SQL, filtrovat a vizualizovat data uložená v úložišti objektů Blob v Azure. Hello Scala fragmenty kódu v tomto článku s hello řešení, které zobrazit relevantní pozemků hello toovisualize hello data spustit v nainstalovaná na clustery Spark hello poznámkové bloky Jupyter. Hello modelování kroky v těchto tématech mít kódu této ukazuje, jak tootrain, vyhodnotit, uložit a používat každý typ modelu.

Postup instalace Hello a kódu v tomto článku jsou pro Azure HDInsight 3.4 Spark 1.6. Ale hello kódu v tomto článku a v hello [poznámkového bloku Jupyter Scala](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) obecné a by měla fungovat v jakémkoliv clusteru Spark. nastavení clusteru s podporou Hello a kroky správy může být mírně lišit od co se zobrazí v tomto článku, pokud nepoužíváte HDInsight Spark.

> [!NOTE]
> Téma, které ukazuje, jak toouse Python, nikoli Scala toocomplete úloh na začátku do konce proces vědecké zpracování dat, najdete v části [vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md).
> 
> 

## <a name="prerequisites"></a>Požadavky
* Musíte mít předplatné Azure. Pokud jste již nemají, [získat bezplatnou zkušební verzi Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Je nutné hello toocomplete clusteru Azure HDInsight 3.4 Spark 1.6 následující postupy. toocreate clusteru, najdete v pokynech hello v [Začínáme: Vytvořte Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Nastavit hello clusteru typ a verze na hello **vybrat typ clusteru** nabídky.

![Konfigurace typu clusteru HDInsight](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

Popis hello NYC taxíkem cestě dat a pokyny, jak tooexecute kód z poznámkového bloku Jupyter v clusteru Spark hello najdete v tématu hello příslušné části v [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md).  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a>Spustí kód Scala z poznámkového bloku Jupyter v clusteru Spark hello
Můžete spustit poznámkového bloku Jupyter z hello portálu Azure. Najděte hello cluster Spark na řídicím panelu a klikněte na něj tooenter hello správu stránky pro váš cluster. Klikněte na tlačítko **řídicí panely clusteru**a potom klikněte na **Poznámkový blok Jupyter** tooopen hello poznámkového bloku spojené s clusterem Spark hello.

![Řídicí panel clusteru a poznámkové bloky Jupyter](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Také můžete přístup poznámkové bloky Jupyter na https://&lt;clustername&gt;.azurehdinsight.net/jupyter. Nahraďte *clustername* s hello názvem vašeho clusteru. Potřebujete hello heslo pro váš správce účtu tooaccess hello poznámkové bloky Jupyter.

![Přejděte poznámkových bloků tooJupyter pomocí názvu clusteru hello](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Vyberte **Scala** toosee adresář, který má několik příkladů hotových poznámkových bloků, že použití hello PySpark rozhraní API. Hello modelování zkoumání a hodnocení pomocí poznámkového bloku Scala.ipynb, která obsahuje ukázky kódu hello této sady témat Spark je k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).

Můžete nahrát poznámkového bloku hello přímo z Githubu toohello Poznámkový blok Jupyter serveru v clusteru Spark. Na domovské stránce Jupyter, klikněte na tlačítko hello **nahrát** tlačítko. V Průzkumníku souborů hello, vložte adresu URL webu GitHub (nezpracovaná obsah) hello hello Scala poznámkového bloku a pak klikněte na tlačítko **otevřete**. Poznámkový blok Scala Hello je k dispozici na hello následující adresu URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Instalace: Kontexty přednastavených Spark a Hive, Spark Magic a knihoven Spark
### <a name="preset-spark-and-hive-contexts"></a>Předvolby kontexty Spark a Hive
    # SET hello START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Hello Spark jádra, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontexty. Kontexty Spark nebo Hive sady hello tooexplicitly nepotřebujete před zahájením práce s hello aplikací, které vyvíjíte. Hello přednastavené kontexty jsou:

* `sc`pro SparkContext
* `sqlContext`pro HiveContext

### <a name="spark-magics"></a>Spark Magic
Hello Spark jádra poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s `%%`. Dva z těchto příkazů se používají v hello následující ukázky kódu.

* `%%local`Určuje, že kód hello v dalších řádcích bude proveden místně. Hello kód musí být platný kód Scala.
* `%%sql -o <variable name>`provede dotaz Hive proti `sqlContext`. Pokud hello `-o` parametr se předává, hello výsledek dotazu hello je uchován v hello `%%local` Scala kontextu jako snímek dat Spark.

Pro další informace o hello jádra pro poznámkové bloky Jupyter a jejich předdefinované "magics", volání s `%%` (například `%%local`), najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

### <a name="import-libraries"></a>Importovat knihovny
Importovat hello Spark, MLlib a další knihovny, které budete potřebovat pomocí hello následující kód.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Přijímání dat
Hello prvním krokem v procesu vědecké zpracování dat hello je tooingest hello dat, které chcete tooanalyze. Přepnutím hello data z externích zdrojů nebo systémy němž je umístěna do vašeho prostředí zkoumání a modelování data. V tomto článku hello data, která jste ingestování je připojený k ukázka 0,1 % souboru hello taxíkem služební cestě a tarif (uložený jako soubor TSV). prostředí pro zkoumání a modelování datového Hello je Spark. Tato část obsahuje hello kód toocomplete hello následující řadu úloh:

1. Nastavit cesty adresáře pro data a modelu úložiště.
2. Přečtěte si v hello vstupní datové sady (uložený jako soubor TSV).
3. Definujte schéma pro hello data a vyčištění hello data.
4. Vytvořte rámeček vyčištěnými dat a uložení do mezipaměti v paměti.
5. Zaregistrujte hello data jako do dočasné tabulky v SQLContext.
6. Dotaz na tabulku hello a importovat hello výsledky do rámečku data.

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Nastavení cesty adresáře pro umístění úložiště v Azure Blob storage
Spark můžete číst a zapisovat tooAzure úložiště objektů Blob. Můžete použít Spark tooprocess stávající data a pak uložte výsledky hello znovu v úložišti objektů Blob.

modely toosave nebo souborů v Blob storage, musíte tooproperly zadejte cestu hello. Referenční dokumentace hello výchozí kontejner připojit toohello Spark cluster pomocí cesty, který začíná `wasb:///`. Referenční jiných umístění pomocí `wasb://`.

Hello následující ukázka kódu určuje umístění hello toobe hello vstupní data pro čtení a hello cesta tooBlob úložiště, které je připojené toohello clusteru Spark pro uložení hello modelu.

    # SET PATHS tooDATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET hello MODEL STORAGE DIRECTORY PATH
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-toohello-schema"></a>Umožňuje importovat data, RDD vytvořit a definovat rámeček dat podle schématu toohello
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello SCHEMA BASED ON hello HEADER OF hello FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING toohello SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE hello CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER hello DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas toorun hello buňky: 8 sekund.

### <a name="query-hello-table-and-import-results-in-a-data-frame"></a>Dotaz na tabulku hello a importovat výsledky v rámci dat
Další tabulka hello dotazu pro tarif, osobní a tip data; filtrování dat poškozen a odlehlé; a tisk několik řádků.

    # QUERY hello DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY hello TOP THREE ROWS
    sqlResultsDF.show(3)

**Výstup:**

| fare_amount | passenger_count | tip_amount | vysypávány |
| --- | --- | --- | --- |
|        13.5 |1.0 |2.9 |1.0 |
|        16.0 |2.0 |3.4 |1.0 |
|        10.5 |2.0 |1.0 |1.0 |

## <a name="data-exploration-and-visualization"></a>Zkoumání dat a vizualizaci
Po přepnutí hello data do Spark, hello dalším krokem v hello proces vědecké zpracování dat je toogain podrobnější vysvětlení hello dat prostřednictvím zkoumání a vizualizace. V této části Zkontrolujte hello taxíkem dat pomocí dotazů SQL. Potom hello výsledky importu do tooplot rámečkem data hello cílových proměnných a potenciální funkcí pro visual kontroly pomocí funkce Automatické vizualizace hello Jupyter.

### <a name="use-local-and-sql-magic-tooplot-data"></a>Použít místní a magic tooplot dat SQL
Ve výchozím nastavení je k dispozici v rámci kontextu hello hello relace, který je na hello pracovní uzly trvalý výstup hello žádné fragment kódu, který lze spustit ze poznámkového bloku Jupyter. Pokud chcete, aby toosave služební cestě toohello pracovním uzlům pro každý výpočty a pokud všechny hello data, která je nutné pro vaše výpočetní je k dispozici místně na uzel serveru Jupyter hello (což je hello hlavního uzlu), můžete použít hello `%%local` kouzelná toorun hello kódu fragment kódu na serveru Jupyter hello.

* **SQL magic** (`%%sql`). Hello HDInsight Spark jádra podporuje dotazy na snadno vložené HiveQL pro SQLContext. Hello (`-o VARIABLE_NAME`) argument potrvají výstup hello dotazu SQL hello jako rámeček Pandas dat na serveru Jupyter hello. To znamená, že budete mít k dispozici v místním režimu hello.
* `%%local`**magic**. Hello `%%local` magic běží kód hello místně na serveru Jupyter hello, což je hello hlavního uzlu v clusteru HDInsight hello. Obvykle použijete, `%%local` magic ve spojení s hello `%%sql` magic s hello `-o` parametr. Hello `-o` parametr by zachovat hello výstup hello dotazu SQL místně a potom `%%local` magic by aktivovat hello další sadu toorun fragmentu kódu místně proti výstup hello hello dotazů SQL, který je místně trvalé.

### <a name="query-hello-data-by-using-sql"></a>Dotaz na data hello pomocí SQL
Tento dotaz načte služebních cest taxíkem hello velikost tarif, osobní počet a velikost tip.

    # RUN hello SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

V následujícím kódu hello, hello `%%local` magic vytvoří místní data rámce, sqlResults. Můžete sqlResults tooplot pomocí matplotlib.

> [!TIP]
> Místní magic se používá více než jednou. v tomto článku. Pokud je velké datové sady, prosím ukázkové toocreate dat rámce, který můžete začlenit do místní paměti.
> 
> 

### <a name="plot-hello-data"></a>Vykreslení dat hello
Můžete zobrazit pomocí kód Python po lokální kontext jako snímek dat Pandas hello datové rámce.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES.
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Hello Spark jádra automaticky vizualizuje výstup hello dotazů SQL (HiveQL), po spuštění kódu hello. Můžete si vybrat mezi několik typů vizualizace:

* Table
* Výsečový
* Perokresba
* Oblast
* Panel

Tady je dat hello tooplot hello kódu:

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Výstup:**

![Tip velikost histogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tip velikost podle velikosti tarif](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Vytvoření funkce transformace funkce a potom připravená data data pro vstup do funkce modelování
Pro funkce na základě stromu modelování ze Spark ML a MLlib máte s využitím různých technik, jako například přihrádkování, indexování, jeden horkou kódování a vectorization tooprepare cíle a funkce. Zde jsou toofollow hello postupy v této části:

1. Vytvořit novou funkci ve **přihrádkování** čas do provozu časové intervaly.
2. Použít **horkou jeden kódování a indexování** toocategorical funkce.
3. **Ukázka a rozdělení hello datové sady** do učení a testovací zlomků.
4. **Zadejte proměnnou školení a funkce**a poté vytvořit indexované nebo horkou jeden kódovaný školení a testování vstupní bod s popiskem odolné distribuovaných datové sady (RDDs) nebo datové rámce.
5. Automaticky **kategorií a vectorize funkce a cíle** toouse jako vstupy pro modely machine learning.

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Vytvořit novou funkci přihrádkování čas do provozu čas intervalů
Tento kód ukazuje, jak toocreate novou funkci ve přihrádkování čas do provozu čas intervalů a jak toocache hello výsledné datové rámce v paměti. Pokud rámce RDDs a data se používají opakovaně, ukládání do mezipaměti vede tooimproved časy spuštění. Podle toho budete mezipaměti rámce RDDs a data v několika fázích v hello následující postupy.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE hello DATA FRAME IN MEMORY AND MATERIALIZE hello DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indexování a jeden horkou kódování kategorií funkcí
Hello modelování a předvídání, funkce MLlib potřeba funkce s kategorií vstupní data toobe indexované nebo kódovaný předchozí toouse. V této části se dozvíte, jak tooindex nebo kódování kategorií funkce pro vstup do funkce modelování hello.

Třeba tooindex nebo kódování modely různými způsoby v závislosti na modelu hello. Například logistic a lineární regrese modely vyžadovat horkou jeden kódování. Například můžete do tři sloupce funkce rozšířit funkce s tří kategorií. Každý sloupec obsahuje 0 nebo 1 v závislosti na kategorii hello pozorování. MLlib poskytuje hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce pro jeden horkou kódování. Tato kodér mapuje sloupec popisek indexy tooa sloupce binární vektorů s maximálně jednu jeden – hodnotu. Toto kódování algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression, může být použité toocategorical funkce.

Zde transformace příklady tooshow pouze čtyři proměnné, které jsou řetězce znaků. Další proměnné, jako například den v týdnu, reprezentována číselné hodnoty, jako kategorií proměnné také mohou indexu.

Pro indexování, použijte `StringIndexer()`a pro jeden horkou kódování, použijte `OneHotEncoder()` funkce z MLlib. Tady je hello tooindex kódu a kódování kategorií funkce:

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas toorun hello buňky: 4 sekundy.

### <a name="sample-and-split-hello-data-set-into-training-and-test-fractions"></a>Ukázka a rozdělení hello do zlomků učení a testovací datové sady
Tento kód vytvoří náhodné vzorky hello dat (v tomto příkladu 25 %). I když vzorkování není potřeba v tomto příkladu kvůli toohello velikost hello datových sad, hello článek ukazuje, jak můžete zkusit, abyste věděli, jak toouse pro vlastní problémy v případě potřeby. Po velká vzorky lze ušetřit čas významné během cvičení modelů. Ukázka hello dále rozdělením školení část (v tomto příkladu 75 %) a testování částí (v tomto příkladu 25 %) toouse v klasifikaci a regresní modelování.

Přidejte řádek tooeach náhodné číslo (mezi 0 a 1) (ve sloupci "rand –"), může být použité tooselect křížové ověření složení během cvičení.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT hello SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas toorun hello buňky: 2 sekundy.

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Zadejte proměnnou školení a funkce a pak vytvořte indexované nebo jeden horkou kódovaný trénování a testování vstup s názvem bez přípony bodu RDDs nebo data rámce
Tato část obsahuje kód, který ukazuje, jak tooindex kategorií textová data jako s popiskem typ datového bodu a zakódovat je, abyste mohli používat, tootrain a testování MLlib logistic regression a jinými modely klasifikace. S popiskem bodu objekty jsou RDDs naformátované způsobem, který je nutný jako vstupní data pro většinu v MLlib algoritmů strojového učení. A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.

V tento kód zadejte hello Cílová proměnná (závislé) a hello funkce toouse tootrain modelů. Pak vytvoříte indexované nebo jeden horkou kódovaný trénování a testování vstup s názvem bez přípony bodu RDDs nebo data rámce.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY hello TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas toorun hello buňky: 4 sekundy.

### <a name="automatically-categorize-and-vectorize-features-and-targets-toouse-as-inputs-for-machine-learning-models"></a>Automaticky kategorií a vectorize funkce a cíle toouse jako vstupy pro modely machine learning
Použijte toouse Spark ML toocategorize hello cíle a funkce na základě stromu modelování funkcí. Kód Hello dokončení dvě úlohy:

* Vytvoří binární cíl pro klasifikaci pomocí prahovou hodnotu 0,5 přiřazení hodnotu 0 nebo 1 datový bod tooeach mezi 0 a 1.
* Automaticky rozděluje funkce. Pokud hello počet jedinečných hodnot na číselné u všech funkcí je menší než 32, je zařazený do kategorie této funkce.

Tady je hello kód pro tyto dvě úlohy.

    # CATEGORIZE FEATURES AND BINARIZE hello TARGET FOR hello BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR hello REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Model binární klasifikace: předpovědět, zda by měl být placené tip
V této části můžete vytvořit tři typy toopredict modely binární klasifikace, jestli by měl být placené tip:

* A **logistic regresní model** pomocí hello Spark ML `LogisticRegression()` – funkce
* A **model klasifikace náhodných doménové struktury** pomocí hello Spark ML `RandomForestClassifier()` – funkce
* A **přechodu zvýšení skóre modelu klasifikace stromu** pomocí hello MLlib `GradientBoostedTrees()` – funkce

### <a name="create-a-logistic-regression-model"></a>Vytvoření modelu logistic regression
Dále vytvořte logistic regresní model pomocí hello Spark ML `LogisticRegression()` funkce. Vytvoříte model hello vytváření kódu v sérii kroků:

1. **Train hello model** dat pomocí jednu sadu parametrů.
2. **Vyhodnocení modelu hello** na testovací datové sady s metriky.
3. **Uložit hello model** v úložišti objektů Blob pro budoucí spotřeby.
4. **Určení skóre modelu hello** proti testovacích datech.
5. **Vykreslení hello výsledky** s příjemce operační křivek vlastnosti (ROC).

Tady je hello kód pro tyto postupy:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON hello TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE hello MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Zatížení, stanovení skóre a uložte výsledky hello.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD hello SAVED MODEL AND SCORE hello TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON hello TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC RESULTS
    println("ROC on test data = " + ROC)


**Výstup:**

ROC na testovací data = 0.9827381497557599

Na místní Pandas dat rámce tooplot hello: křivka ROC použijte jazyk Python.

    # QUERY hello RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT hello ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT hello ROC CURVE
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


**Výstup:**

![Tip nebo žádné křivka ROC tipu](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a>Vytvoření modelu klasifikace náhodných doménové struktury
Dále vytvořte model klasifikace náhodných doménové struktury pomocí hello Spark ML `RandomForestClassifier()` fungovat a pak vyhodnotit hello modelu na testovacích datech.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE hello RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT hello MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE hello MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Výstup:**

ROC na testovací data = 0.9847103571552683

### <a name="create-a-gbt-classification-model"></a>Vytvoření modelu GBT klasifikace
Dále vytvořte klasifikaci model GBT pomocí MLlib na `GradientBoostedTrees()` fungovat a pak vyhodnotit hello modelu na testovacích datech.

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN hello MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE hello MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE hello MODEL ON TEST INSTANCES AND hello COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS tooEVALUATE hello MODEL ON hello TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Výstup:**

Oblasti v rámci křivka ROC: 0.9846895479241554

## <a name="regression-model-predict-tip-amount"></a>Regresní model: předpovědi velikost tipu
V této části vytvoříte dva typy regrese modely toopredict hello tip velikost:

* A **model lineární regrese Vyřešeno** pomocí hello Spark ML `LinearRegression()` funkce. Můžete uložit hello modelu a vyhodnocení modelu hello na testovací data.
* A **zvyšovat skóre přechodu stromu regresní model** pomocí hello Spark ML `GBTRegressor()` funkce.

### <a name="create-a-regularized-linear-regression-model"></a>Vytvořit model lineární regrese Vyřešeno
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING hello SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT hello MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE hello MODEL OVER hello TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE hello MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT hello COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas toorun hello buňky: 13 sekund.

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("R-sqr on test data = " + r2)


**Výstup:**

R – sqr na testovací data = 0.5960320470835743

V dalším kroku dotazu hello výsledky testu jako data snímku a jeho použití AutoVizWidget a matplotlib toovisualize ho.

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Hello kód vytvoří rámečku místní data z dotazu výstup hello a zobrazuje hello data. Hello `%%local` magic vytvoří místní data rámce, `sqlResults`, které můžete použít tooplot s matplotlib.

> [!NOTE]
> Tato magic Spark se používá více než jednou. v tomto článku. Pokud je velká hello množství dat, by měl ukázkové toocreate dat rámce, který můžete začlenit do místní paměti.
> 
> 

Vytvořte pozemků pomocí Python matplotlib.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT hello RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Výstup:**

![Tip velikost: skutečnost a předpokládaných](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a>Vytvoření GBT regresní model
Vytvoření GBT regresní model pomocí hello Spark ML `GBTRegressor()` fungovat a pak vyhodnotit hello modelu na testovacích datech.

[Boosted přechodu stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy. GBTs train rozhodovací stromy interaktivně toominimize funkci ztrátu. GBTs můžete použít pro regresní a klasifikace. Se může zpracovat kategorií funkce, nevyžadují funkce škálování a můžete zaznamenat nonlinearities a interakce funkce. Také můžete je v nastavení multiclass klasifikace.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("Test R-sqr is: " + Test_R2);


**Výstup:**

Je test R-sqr: 0.7655383534596654

## <a name="advanced-modeling-utilities-for-optimization"></a>Pokročilé modelování nástrojů pro optimalizaci
V této části použijte nástroje learning počítače, které vývojáři často používají pro optimalizaci modelu. Konkrétně můžete pomocí parametru (vymetání) komínů a křížové ověření optimalizovat modely machine learning třemi různými způsoby:

* Rozdělení dat hello do vlaku a ověření nastaví optimalizovat hello modelu s použitím technologie hyper parametr (vymetání) komínů na trénovací sady a vyhodnocení na sadu ověření (lineární regrese)
* Optimalizovat hello modelu s použitím křížové ověření a technologie hyper parametr komínů pomocí funkce CrossValidator Spark ML (binární klasifikace)
* Optimalizovat hello modelu s použitím vlastní kód křížové ověření a parametr sweeping toouse žádné strojového učení sadu funkce a parametr (lineární regrese)

**Křížové ověření** je technika, který vyhodnocuje, jak dobře model trénink na známé sadu dat bude generalize toopredict hello funkce datové sady, na kterých je nebyla cvičena. Hello obecnou představu za tato technika je, že cvičení modelu na datové sady známých dat, a pak hello přesnost jeho předpovědi je testován vůči nezávislé datové sady. Běžná implementace je toodivide datové sady do *tisíc*-složení a potom trénování modelu hello v kruhového dotazování na všechny kromě jednoho hello složení.

**Technologie Hyper parametr optimalizace** potížím hello vybrat sadu technologie hyper parametrů pro algoritmu učení, obvykle s cílem hello optimalizace měření výkonu hello algoritmus na nezávislé datové sady. Technologie hyper parametr je hodnota, která je nutné zadat mimo hello modelu školení postupu. Předpoklady o technologie hyper parametr hodnoty může ovlivnit hello flexibilitu a přesnosti modelu hello. Rozhodovací stromy mít technologie hyper parametry, například jako hello potřeby hloubkou a počet nechá ve stromu hello. Je nutné nastavit termín snížení chybnou klasifikaci pro podporu vektoru počítač (SVM).

Běžné optimalizace hyper parametr tooperform způsob, jak je toouse vyhledávání mřížky, označované taky jako **oblouku parametr**. V hledání mřížky provádí podrobné prohledávání prostřednictvím hello hodnoty zadané podmnožinu hello hyper parametr místa pro algoritmus učení. Křížového ověření může poskytovat metriky toosort out hello optimální výsledky vyprodukované hello mřížky vyhledávacího algoritmu výkonu. Pokud používáte křížové ověření (technologie hyper parametr vymetání) komínů, můžete pomoct limit problémy jako overfitting tootraining datový model. Tímto způsobem hello modelu zachová hello kapacity tooapply toohello obecné sady dat, ze které hello jste extrahovali Cvičná data.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Optimalizace model lineární regrese s hyper parametr (vymetání) komínů
Dále rozdělit data do vlaku a ověření sady, použití technologie hyper parametr komínů na školení nastavit toooptimize hello modelu a vyhodnocení na sadu ověření (lineární regrese).

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE hello ESTIMATOR FUNCTION: `hello LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE hello PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET), AND THEN hello SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Výstup:**

Je test R-sqr: 0.6226484708501209

### <a name="optimize-hello-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Optimalizovat hello binární klasifikace modelu s použitím (vymetání) křížové ověření a technologie hyper parametr komínů
Tato část uvádí, jak toooptimize binární klasifikace modelu s použitím sweeping křížové ověření a technologie hyper parametr. Tato služba využívá hello Spark ML `CrossValidator` funkce.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS tooUSE WITH hello TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE hello ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY hello NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE hello TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE hello TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Výstup:**

Čas toorun hello buňky: 33 sekund.

### <a name="optimize-hello-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Optimalizovat model lineární regrese hello s použitím vlastní křížové ověření a parametr sweeping kód
V dalším kroku optimalizovat hello modelu s použitím vlastního kódu a identifikovat nejlepší parametry modelu hello pomocí hello kritéria nejvyšší přesnost. Pak vytvořte hello konečné modelu, hodnocení hello modelu na testovací data a uložit hello modelu v úložišti objektů Blob. Nakonec načíst hello model, stanovení skóre testovací data a vyhodnotit přesnost.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello PARAMETER GRID AND hello NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY hello NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND hello PARAMETER GRID tooGET AND IDENTIFY hello BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 too(nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 too(numModels-1)) {
            for (nParams <- 0 too(numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET hello BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 too(numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE hello BEST MODEL WITH hello BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE hello BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON hello TRAINING SET WITH hello BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


    # LOAD hello MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST hello MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Výstup:**

Čas toorun hello buňky: 61 sekund.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Využívat modelů learning vytvořené Spark počítač automaticky pomocí Scala
Přehled témata, která vás provede procesem hello úlohy, které tvoří hello procesu vědecké zpracování dat v Azure najdete v tématu [proces vědecké účely dat Team](http://aka.ms/datascienceprocess).

[Tým datové vědy proces návody](data-science-process-walkthroughs.md) popisuje další návody začátku do konce, které ukazují hello kroky hello Team datové vědy proces pro konkrétní scénáře. návody Hello také ilustrují, jak toocombine cloudové a místní nástrojů a služeb do pracovního postupu nebo kanálu toocreate inteligentního aplikace.

[Stanovení skóre modely vytvořené Spark strojové učení](machine-learning-data-science-spark-model-consumption.md) se dozvíte, jak toouse Scala kód tooautomatically načíst a stanovíte jeho skóre nové sady dat s modely machine learning součástí Spark a uloží do úložiště objektů Blob v Azure. Můžete podle podle hello pokynů existuje a jednoduše místo hello kód Python Scala kód v tomto článku automatizované spotřeby.

