---
title: "aaaBuild Apache Spark strojového učení aplikace v Azure HDInsight | Microsoft Docs"
description: "Podrobný návod, jak toobuild Apache Spark strojového učení aplikace na HDInsight Spark clusterů pomocí poznámkového bloku Jupyter"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 332bd89876f7ebf178f7573d6018d064edfe9a8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Sestavení aplikace strojového učení Apache Spark v Azure HDInsight

Zjistěte, jak toobuild aplikaci Apache Spark machine learning pomocí Spark clusteru v HDInsight. Tento článek ukazuje, jak toouse hello poznámkového bloku Jupyter s toobuild hello clusteru k dispozici a testování této aplikace. aplikace Hello používá hello ukázková HVAC.csv data, která je k dispozici na všech clusterech ve výchozím nastavení.

**Požadavky:**

Musíte mít následující hello:

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Pochopení hello datové sady
Před můžeme začít vytvářet aplikace hello, dejte nám porozumět hello struktura hello dat, pro kterou jsme sestavení aplikace hello a hello druh analýzy, kterou uděláme hello data. 

V tomto článku používáme hello ukázka **HVAC.csv** datový soubor, který je k dispozici v hello účet úložiště Azure, které přidružené hello clusteru HDInsight. V rámci účtu úložiště hello, hello soubor je v **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Stáhněte si a nainstalujte tooget soubor CSV hello snímek dat hello.  

![Snímek dat použít Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "snímek dat použít například Spark machine learning")

Hello data zobrazují hello cíl teploty a skutečný teploty hello vytváření, který má nainstalované systémy VZT. Předpokládejme hello **systému** sloupec představuje hello systému ID a hello **SystemAge** sloupec představuje hello počet roků hello TVK systému byl na místě v sestavení hello.

Tato data toopredict používáme, zda v budově bude hotter nebo colder na základě na hello cíl teploty, daného ID systému a stáří systému.

## <a name="app"></a>Zápis aplikací Spark machine learning pomocí Spark MLlib
V této aplikaci používáme tooperform kanálu Spark ML klasifikaci dokumentů. V kanálu hello jsme hello dokumentu rozdělení do slova, převést hello slova číselné funkce vector a nakonec sestavení modelu předpovědi pomocí hello funkce vektory a popisky. Proveďte následující kroky toocreate hello aplikace hello.

1. Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.   
2. Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.
   
   > [!NOTE]
   > Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Vytvořte nový poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.
   
    ![Vytvoření poznámkového bloku Jupyter Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "vytvoření poznámkového bloku Jupyter například Spark machine learning")
4. Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.
   
    ![Zadejte název poznámkového bloku Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "zadejte název poznámkového bloku například Spark machine learning")
5. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně. Hello kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu hello. Můžete začít importem hello typy, které jsou požadovány pro tento scénář. Vložte následující fragment kódu do prázdné buňky hello a potom stiskněte klávesu **SHIFT + ENTER**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Musí nyní načíst data hello (hvac.csv), jeho analyzovat a použít ho tootrain hello modelu. V takovém případě definujete funkci, která zkontroluje, zda je větší než hello cíl teploty hello skutečné teploty vytváření hello. Pokud skutečný teploty hello je vyšší, vytváření hello je aktivní, označené hodnotou hello **1.0**. Pokud skutečný teploty hello je menší, vytváření hello je studenou, označené hodnotou hello **0,0**. 
   
    Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.

        # List hello structure of data for better understanding. Because hello data will be
        # loaded as an array, this structure makes it easy toounderstand what each element
        # in hello array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses hello raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load hello raw HVAC.csv file, parse it using hello function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Konfiguraci kanálu hello Spark machine learning, která se skládá ze tří fází: tokenizátor, hashingTF a lr. Další informace o co je kanál, a jak to funguje najdete <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning kanálu</a>.
   
    Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Umístit hello kanálu toohello školení dokument. Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        model = pipeline.fit(training)
3. Ověřte hello školení dokumentu toocheckpoint průběh s aplikací hello. Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        training.show()
   
    To by měly poskytnout hello výstup podobný toohello následující:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Přejděte zpět a ověřte výstup hello proti hello nezpracovaná souboru CSV. Například soubor CSV hello první řádek hello má tato data:

    ![Výstupní data snímku Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "výstupní data snímku například Spark machine learning")

    Všimněte si, jak skutečné teploty hello je menší než teploty cíl hello k vytváření hello je cold. Proto ve výstupu školení hello hello hodnotu **popisek** v hello je první řádek **0,0**, což znamená budova hello není aktivní.

1. Připravte datové sady toorun hello modulu trained model proti. toodo tedy jsme by předat na ID systému a stáří systému (označené jako **SystemInfo** ve výstupu školení hello), a by předpovědi modelu hello tom, zda text hello sestavování s použitím tohoto věku ID a systému systému by být hotter (označen 1.0) nebo studenější ( odlišené 0,0).
   
   Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Nakonec provádějte předpovědi na hello testovacích datech. Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Měli byste vidět výstup podobný toohello následující:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   Z hello první řádek v hello předpovědi, uvidíte, že pro systém TVK s ID 20 a stáří systému 25 let hello vytváření bude aktivní (**předpovědi = 1.0**). první hodnota Hello DenseVector (0.49999) odpovídá toohello předpovědi 0,0 a druhá hodnota hello (0.5001) odpovídá toohello předpovědi 1.0. Ve výstupu hello, i když hello druhá hodnota, která je jen málo vyšší, zobrazuje hello modelu **předpovědi = 1.0**.
4. Po dokončení spuštění aplikace hello, měli byste vypnout hello poznámkového bloku toorelease hello prostředky. toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**. Dojde k vypnutí a zavřít hello Poznámkový blok.

## <a name="anaconda"></a>Použít Anaconda scikit-další knihovny pro Spark machine learning
Clustery Apache Spark v HDInsight zahrnují knihovnami Anaconda. To zahrnuje také hello **scikit-Další** knihoven pro machine learning. Knihovna Hello také obsahuje různé datové sady, které můžete použít toobuild ukázkové aplikace přímo z poznámkového bloku Jupyter. Pro příklady použití hello scikit-další knihovny najdete [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
