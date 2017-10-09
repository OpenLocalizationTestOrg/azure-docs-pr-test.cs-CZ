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
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a><span data-ttu-id="f42ab-103">Sestavení aplikace strojového učení Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-103">Build Apache Spark machine learning applications on Azure HDInsight</span></span>

<span data-ttu-id="f42ab-104">Zjistěte, jak toobuild aplikaci Apache Spark machine learning pomocí Spark clusteru v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f42ab-104">Learn how toobuild an Apache Spark machine learning application using a Spark cluster on HDInsight.</span></span> <span data-ttu-id="f42ab-105">Tento článek ukazuje, jak toouse hello poznámkového bloku Jupyter s toobuild hello clusteru k dispozici a testování této aplikace.</span><span class="sxs-lookup"><span data-stu-id="f42ab-105">This article shows how toouse hello Jupyter notebook available with hello cluster toobuild and test this application.</span></span> <span data-ttu-id="f42ab-106">aplikace Hello používá hello ukázková HVAC.csv data, která je k dispozici na všech clusterech ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f42ab-106">hello application uses hello sample HVAC.csv data that is available on all clusters by default.</span></span>

<span data-ttu-id="f42ab-107">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="f42ab-107">**Prerequisites:**</span></span>

<span data-ttu-id="f42ab-108">Musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="f42ab-108">You must have hello following:</span></span>

* <span data-ttu-id="f42ab-109">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f42ab-109">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="f42ab-110">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="f42ab-110">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> 

## <span data-ttu-id="f42ab-111"><a name="data"></a>Pochopení hello datové sady</span><span class="sxs-lookup"><span data-stu-id="f42ab-111"><a name="data"></a>Understand hello data set</span></span>
<span data-ttu-id="f42ab-112">Před můžeme začít vytvářet aplikace hello, dejte nám porozumět hello struktura hello dat, pro kterou jsme sestavení aplikace hello a hello druh analýzy, kterou uděláme hello data.</span><span class="sxs-lookup"><span data-stu-id="f42ab-112">Before we start building hello application, let us understand hello structure of hello data for which we build hello application and hello kind of analysis we will do on hello data.</span></span> 

<span data-ttu-id="f42ab-113">V tomto článku používáme hello ukázka **HVAC.csv** datový soubor, který je k dispozici v hello účet úložiště Azure, které přidružené hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f42ab-113">In this article, we use hello sample **HVAC.csv** data file that is available in hello Azure Storage account that you associated with hello HDInsight cluster.</span></span> <span data-ttu-id="f42ab-114">V rámci účtu úložiště hello, hello soubor je v **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-114">Within hello storage account, hello file is at **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span> <span data-ttu-id="f42ab-115">Stáhněte si a nainstalujte tooget soubor CSV hello snímek dat hello.</span><span class="sxs-lookup"><span data-stu-id="f42ab-115">Download and open hello CSV file tooget a snapshot of hello data.</span></span>  

<span data-ttu-id="f42ab-116">![Snímek dat použít Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "snímek dat použít například Spark machine learning")</span><span class="sxs-lookup"><span data-stu-id="f42ab-116">![Snapshot of data used for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Snapshot of data used for Spark machine learning example")</span></span>

<span data-ttu-id="f42ab-117">Hello data zobrazují hello cíl teploty a skutečný teploty hello vytváření, který má nainstalované systémy VZT.</span><span class="sxs-lookup"><span data-stu-id="f42ab-117">hello data shows hello target temperature and hello actual temperature of a building that has HVAC systems installed.</span></span> <span data-ttu-id="f42ab-118">Předpokládejme hello **systému** sloupec představuje hello systému ID a hello **SystemAge** sloupec představuje hello počet roků hello TVK systému byl na místě v sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="f42ab-118">Let's assume hello **System** column represents hello system ID and hello **SystemAge** column represents hello number of years hello HVAC system has been in place at hello building.</span></span>

<span data-ttu-id="f42ab-119">Tato data toopredict používáme, zda v budově bude hotter nebo colder na základě na hello cíl teploty, daného ID systému a stáří systému.</span><span class="sxs-lookup"><span data-stu-id="f42ab-119">We use this data toopredict whether a building will be hotter or colder based on hello target temperature, given a system ID and system age.</span></span>

## <span data-ttu-id="f42ab-120"><a name="app"></a>Zápis aplikací Spark machine learning pomocí Spark MLlib</span><span class="sxs-lookup"><span data-stu-id="f42ab-120"><a name="app"></a>Write a Spark machine learning application using Spark MLlib</span></span>
<span data-ttu-id="f42ab-121">V této aplikaci používáme tooperform kanálu Spark ML klasifikaci dokumentů.</span><span class="sxs-lookup"><span data-stu-id="f42ab-121">In this application we use a Spark ML pipeline tooperform a document classification.</span></span> <span data-ttu-id="f42ab-122">V kanálu hello jsme hello dokumentu rozdělení do slova, převést hello slova číselné funkce vector a nakonec sestavení modelu předpovědi pomocí hello funkce vektory a popisky.</span><span class="sxs-lookup"><span data-stu-id="f42ab-122">In hello pipeline, we split hello document into words, convert hello words into a numerical feature vector, and finally build a prediction model using hello feature vectors and labels.</span></span> <span data-ttu-id="f42ab-123">Proveďte následující kroky toocreate hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f42ab-123">Perform hello following steps toocreate hello application.</span></span>

1. <span data-ttu-id="f42ab-124">Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="f42ab-124">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="f42ab-125">Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-125">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="f42ab-126">Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-126">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="f42ab-127">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f42ab-127">If prompted, enter hello admin credentials for hello cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f42ab-128">Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f42ab-128">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="f42ab-129">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="f42ab-129">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. <span data-ttu-id="f42ab-130">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="f42ab-130">Create a new notebook.</span></span> <span data-ttu-id="f42ab-131">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-131">Click **New**, and then click **PySpark**.</span></span>
   
    <span data-ttu-id="f42ab-132">![Vytvoření poznámkového bloku Jupyter Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "vytvoření poznámkového bloku Jupyter například Spark machine learning")</span><span class="sxs-lookup"><span data-stu-id="f42ab-132">![Create a Jupyter notebook for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Create a Jupyter notebook for Spark machine learning example")</span></span>
4. <span data-ttu-id="f42ab-133">Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="f42ab-133">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="f42ab-134">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="f42ab-134">Click hello notebook name at hello top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="f42ab-135">![Zadejte název poznámkového bloku Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "zadejte název poznámkového bloku například Spark machine learning")</span><span class="sxs-lookup"><span data-stu-id="f42ab-135">![Provide a notebook name for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "Provide a notebook name for Spark machine learning example")</span></span>
5. <span data-ttu-id="f42ab-136">Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně.</span><span class="sxs-lookup"><span data-stu-id="f42ab-136">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="f42ab-137">Hello kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu hello.</span><span class="sxs-lookup"><span data-stu-id="f42ab-137">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="f42ab-138">Můžete začít importem hello typy, které jsou požadovány pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="f42ab-138">You can start by importing hello types that are required for this scenario.</span></span> <span data-ttu-id="f42ab-139">Vložte následující fragment kódu do prázdné buňky hello a potom stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-139">Paste hello following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span> 
   
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
6. <span data-ttu-id="f42ab-140">Musí nyní načíst data hello (hvac.csv), jeho analyzovat a použít ho tootrain hello modelu.</span><span class="sxs-lookup"><span data-stu-id="f42ab-140">You must now load hello data (hvac.csv), parse it, and use it tootrain hello model.</span></span> <span data-ttu-id="f42ab-141">V takovém případě definujete funkci, která zkontroluje, zda je větší než hello cíl teploty hello skutečné teploty vytváření hello.</span><span class="sxs-lookup"><span data-stu-id="f42ab-141">For this, you define a function that checks whether hello actual temperature of hello building is greater than hello target temperature.</span></span> <span data-ttu-id="f42ab-142">Pokud skutečný teploty hello je vyšší, vytváření hello je aktivní, označené hodnotou hello **1.0**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-142">If hello actual temperature is greater, hello building is hot, denoted by hello value **1.0**.</span></span> <span data-ttu-id="f42ab-143">Pokud skutečný teploty hello je menší, vytváření hello je studenou, označené hodnotou hello **0,0**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-143">If hello actual temperature is lesser, hello building is cold, denoted by hello value **0.0**.</span></span> 
   
    <span data-ttu-id="f42ab-144">Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-144">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>

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


1. <span data-ttu-id="f42ab-145">Konfiguraci kanálu hello Spark machine learning, která se skládá ze tří fází: tokenizátor, hashingTF a lr.</span><span class="sxs-lookup"><span data-stu-id="f42ab-145">Configure hello Spark machine learning pipeline that consists of three stages: tokenizer, hashingTF, and lr.</span></span> <span data-ttu-id="f42ab-146">Další informace o co je kanál, a jak to funguje najdete <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning kanálu</a>.</span><span class="sxs-lookup"><span data-stu-id="f42ab-146">For more information about what is a pipeline and how it works see <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pipeline</a>.</span></span>
   
    <span data-ttu-id="f42ab-147">Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-147">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. <span data-ttu-id="f42ab-148">Umístit hello kanálu toohello školení dokument.</span><span class="sxs-lookup"><span data-stu-id="f42ab-148">Fit hello pipeline toohello training document.</span></span> <span data-ttu-id="f42ab-149">Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-149">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        model = pipeline.fit(training)
3. <span data-ttu-id="f42ab-150">Ověřte hello školení dokumentu toocheckpoint průběh s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="f42ab-150">Verify hello training document toocheckpoint your progress with hello application.</span></span> <span data-ttu-id="f42ab-151">Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-151">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        training.show()
   
    <span data-ttu-id="f42ab-152">To by měly poskytnout hello výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="f42ab-152">This should give hello output similar toohello following:</span></span>
   
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

    <span data-ttu-id="f42ab-153">Přejděte zpět a ověřte výstup hello proti hello nezpracovaná souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="f42ab-153">Go back and verify hello output against hello raw CSV file.</span></span> <span data-ttu-id="f42ab-154">Například soubor CSV hello první řádek hello má tato data:</span><span class="sxs-lookup"><span data-stu-id="f42ab-154">For example, hello first row hello CSV file has this data:</span></span>

    <span data-ttu-id="f42ab-155">![Výstupní data snímku Spark machine learning třeba](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "výstupní data snímku například Spark machine learning")</span><span class="sxs-lookup"><span data-stu-id="f42ab-155">![Output data snapshot for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Output data snapshot for Spark machine learning example")</span></span>

    <span data-ttu-id="f42ab-156">Všimněte si, jak skutečné teploty hello je menší než teploty cíl hello k vytváření hello je cold.</span><span class="sxs-lookup"><span data-stu-id="f42ab-156">Notice how hello actual temperature is less than hello target temperature suggesting hello building is cold.</span></span> <span data-ttu-id="f42ab-157">Proto ve výstupu školení hello hello hodnotu **popisek** v hello je první řádek **0,0**, což znamená budova hello není aktivní.</span><span class="sxs-lookup"><span data-stu-id="f42ab-157">Hence in hello training output, hello value for **label** in hello first row is **0.0**, which means hello building is not hot.</span></span>

1. <span data-ttu-id="f42ab-158">Připravte datové sady toorun hello modulu trained model proti.</span><span class="sxs-lookup"><span data-stu-id="f42ab-158">Prepare a data set toorun hello trained model against.</span></span> <span data-ttu-id="f42ab-159">toodo tedy jsme by předat na ID systému a stáří systému (označené jako **SystemInfo** ve výstupu školení hello), a by předpovědi modelu hello tom, zda text hello sestavování s použitím tohoto věku ID a systému systému by být hotter (označen 1.0) nebo studenější ( odlišené 0,0).</span><span class="sxs-lookup"><span data-stu-id="f42ab-159">toodo so, we would pass on a system ID and system age (denoted as **SystemInfo** in hello training output), and hello model would predict whether hello building with that system ID and system age would be hotter (denoted by 1.0) or cooler (denoted by 0.0).</span></span>
   
   <span data-ttu-id="f42ab-160">Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-160">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. <span data-ttu-id="f42ab-161">Nakonec provádějte předpovědi na hello testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="f42ab-161">Finally, make predictions on hello test data.</span></span> <span data-ttu-id="f42ab-162">Hello vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-162">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. <span data-ttu-id="f42ab-163">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="f42ab-163">You should see an output similar toohello following:</span></span>
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   <span data-ttu-id="f42ab-164">Z hello první řádek v hello předpovědi, uvidíte, že pro systém TVK s ID 20 a stáří systému 25 let hello vytváření bude aktivní (**předpovědi = 1.0**).</span><span class="sxs-lookup"><span data-stu-id="f42ab-164">From hello first row in hello prediction, you can see that for an HVAC system with ID 20 and system age of 25 years, hello building will be hot (**prediction=1.0**).</span></span> <span data-ttu-id="f42ab-165">první hodnota Hello DenseVector (0.49999) odpovídá toohello předpovědi 0,0 a druhá hodnota hello (0.5001) odpovídá toohello předpovědi 1.0.</span><span class="sxs-lookup"><span data-stu-id="f42ab-165">hello first value for DenseVector (0.49999) corresponds toohello  prediction 0.0 and hello second value (0.5001) corresponds toohello prediction 1.0.</span></span> <span data-ttu-id="f42ab-166">Ve výstupu hello, i když hello druhá hodnota, která je jen málo vyšší, zobrazuje hello modelu **předpovědi = 1.0**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-166">In hello output, even though hello second value is only marginally higher, hello model shows **prediction=1.0**.</span></span>
4. <span data-ttu-id="f42ab-167">Po dokončení spuštění aplikace hello, měli byste vypnout hello poznámkového bloku toorelease hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="f42ab-167">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="f42ab-168">toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.</span><span class="sxs-lookup"><span data-stu-id="f42ab-168">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="f42ab-169">Dojde k vypnutí a zavřít hello Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="f42ab-169">This will shutdown and close hello notebook.</span></span>

## <span data-ttu-id="f42ab-170"><a name="anaconda"></a>Použít Anaconda scikit-další knihovny pro Spark machine learning</span><span class="sxs-lookup"><span data-stu-id="f42ab-170"><a name="anaconda"></a>Use Anaconda scikit-learn library for Spark machine learning</span></span>
<span data-ttu-id="f42ab-171">Clustery Apache Spark v HDInsight zahrnují knihovnami Anaconda.</span><span class="sxs-lookup"><span data-stu-id="f42ab-171">Apache Spark clusters on HDInsight include Anaconda libraries.</span></span> <span data-ttu-id="f42ab-172">To zahrnuje také hello **scikit-Další** knihoven pro machine learning.</span><span class="sxs-lookup"><span data-stu-id="f42ab-172">This also includes hello **scikit-learn** library for machine learning.</span></span> <span data-ttu-id="f42ab-173">Knihovna Hello také obsahuje různé datové sady, které můžete použít toobuild ukázkové aplikace přímo z poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="f42ab-173">hello library also includes various data sets that you can use toobuild sample applications directly from a Jupyter notebook.</span></span> <span data-ttu-id="f42ab-174">Pro příklady použití hello scikit-další knihovny najdete [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span><span class="sxs-lookup"><span data-stu-id="f42ab-174">For examples on using hello scikit-learn library, see [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span></span>

## <span data-ttu-id="f42ab-175"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="f42ab-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="f42ab-176">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="f42ab-177">Scénáře</span><span class="sxs-lookup"><span data-stu-id="f42ab-177">Scenarios</span></span>
* [<span data-ttu-id="f42ab-178">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="f42ab-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="f42ab-179">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-179">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="f42ab-180">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="f42ab-180">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="f42ab-181">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-181">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="f42ab-182">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="f42ab-182">Create and run applications</span></span>
* [<span data-ttu-id="f42ab-183">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="f42ab-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="f42ab-184">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="f42ab-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="f42ab-185">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="f42ab-185">Tools and extensions</span></span>
* [<span data-ttu-id="f42ab-186">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="f42ab-186">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="f42ab-187">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="f42ab-187">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="f42ab-188">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="f42ab-189">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="f42ab-190">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="f42ab-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="f42ab-191">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="f42ab-191">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="f42ab-192">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="f42ab-192">Manage resources</span></span>
* [<span data-ttu-id="f42ab-193">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-193">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="f42ab-194">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f42ab-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
