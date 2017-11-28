---
title: "aaaMachine učení příklad s MLlib Spark v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Spark MLlib toocreate aplikaci learning počítače, která analyzuje klasifikací prostřednictvím logistic regression datové sady."
keywords: "Spark strojového učení, spark machine learning příklad"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 5c3b83482de5d8fba224398aaafe07fa67ec1fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-mllib-toobuild-a-machine-learning-application-and-analyze-a-dataset"></a><span data-ttu-id="57970-104">Použít Spark MLlib toobuild machine learning aplikací a analýza datové sady</span><span class="sxs-lookup"><span data-stu-id="57970-104">Use Spark MLlib toobuild a machine learning application and analyze a dataset</span></span>

<span data-ttu-id="57970-105">Zjistěte, jak toouse Spark **MLlib** toocreate a strojového učení toodo jednoduché Prediktivní analýza aplikace na otevřete datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="57970-105">Learn how toouse Spark **MLlib** toocreate a machine learning application toodo simple predictive analysis on an open dataset.</span></span> <span data-ttu-id="57970-106">Z Spark předdefinované strojového učení knihovny, tento příklad používá *klasifikace* prostřednictvím logistic regression.</span><span class="sxs-lookup"><span data-stu-id="57970-106">From Spark's built-in machine learning libraries, this example uses *classification* through logistic regression.</span></span> 

> [!TIP]
> <span data-ttu-id="57970-107">V tomto příkladu je také k dispozici jako poznámkový blok Jupyter v clusteru Spark (Linux), který vytvoříte v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57970-107">This example is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="57970-108">Hello poznámkového bloku prostředí vám umožní spustit hello Python fragmenty z Poznámkový blok hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="57970-108">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="57970-109">kurz hello toofollow z v rámci Poznámkový blok, vytvoření Spark clusteru a spouštět poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span><span class="sxs-lookup"><span data-stu-id="57970-109">toofollow hello tutorial from within a notebook, create a Spark cluster and launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span></span> <span data-ttu-id="57970-110">Potom spusťte Poznámkový blok hello **Spark Machine Learning - prediktivní analýzy dat kontroly potravin pomocí MLlib.ipynb** pod hello **Python** složky.</span><span class="sxs-lookup"><span data-stu-id="57970-110">Then, run hello notebook **Spark Machine Learning - Predictive analysis on food inspection data using MLlib.ipynb** under hello **Python** folder.</span></span>
>
>

<span data-ttu-id="57970-111">MLlib je základní knihovna Spark, která poskytuje řadu nástrojů, které jsou užitečné pro úkoly strojového učení, včetně nástroje, které jsou vhodné pro:</span><span class="sxs-lookup"><span data-stu-id="57970-111">MLlib is a core Spark library that provides many utilities useful for machine learning tasks, including utilities that are suitable for:</span></span>

* <span data-ttu-id="57970-112">klasifikace</span><span class="sxs-lookup"><span data-stu-id="57970-112">Classification</span></span>
* <span data-ttu-id="57970-113">Regrese</span><span class="sxs-lookup"><span data-stu-id="57970-113">Regression</span></span>
* <span data-ttu-id="57970-114">Clustering</span><span class="sxs-lookup"><span data-stu-id="57970-114">Clustering</span></span>
* <span data-ttu-id="57970-115">Téma modelování</span><span class="sxs-lookup"><span data-stu-id="57970-115">Topic modeling</span></span>
* <span data-ttu-id="57970-116">Hodnota singulární rozložením (SVD) a analýzu hlavní součásti (PCA)</span><span class="sxs-lookup"><span data-stu-id="57970-116">Singular value decomposition (SVD) and principal component analysis (PCA)</span></span>
* <span data-ttu-id="57970-117">Předpoklad testování a výpočet ukázka statistiky</span><span class="sxs-lookup"><span data-stu-id="57970-117">Hypothesis testing and calculating sample statistics</span></span>

## <a name="what-are-classification-and-logistic-regression"></a><span data-ttu-id="57970-118">Co jsou klasifikace a logistic regression?</span><span class="sxs-lookup"><span data-stu-id="57970-118">What are classification and logistic regression?</span></span>
<span data-ttu-id="57970-119">*Klasifikace*, oblíbených strojového učení úloh, je hello proces řazení do kategorií vstupní data.</span><span class="sxs-lookup"><span data-stu-id="57970-119">*Classification*, a popular machine learning task, is hello process of sorting input data into categories.</span></span> <span data-ttu-id="57970-120">Je hello úlohy z toofigure algoritmus klasifikace jak tooassign "popisků" tooinput data, která zadáte.</span><span class="sxs-lookup"><span data-stu-id="57970-120">It is hello job of a classification algorithm toofigure out how tooassign "labels" tooinput data that you provide.</span></span> <span data-ttu-id="57970-121">Například může úvahách o algoritmu strojového učení, který přijímá uložené informace jako vstup a vydělí hello stock do dvou kategorií: které by měl prodeje a populací, které byste měli mít.</span><span class="sxs-lookup"><span data-stu-id="57970-121">For example, you could think of a machine learning algorithm that accepts stock information as input and divides hello stock into two categories: stocks that you should sell and stocks that you should keep.</span></span>

<span data-ttu-id="57970-122">Logistic regression je hello algoritmus, který používáte pro klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="57970-122">Logistic regression is hello algorithm that you use for classification.</span></span> <span data-ttu-id="57970-123">Spark logistic regression rozhraní API je užitečné pro *binární klasifikace*, nebo do jedné ze dvou skupin klasifikace vstupní data.</span><span class="sxs-lookup"><span data-stu-id="57970-123">Spark's logistic regression API is useful for *binary classification*, or classifying input data into one of two groups.</span></span> <span data-ttu-id="57970-124">Další informace o logistic regresí najdete v tématu [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span><span class="sxs-lookup"><span data-stu-id="57970-124">For more information about logistic regressions, see [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span></span>

<span data-ttu-id="57970-125">V souhrnu, vytváří hello proces logistic regression *logistic funkce* , může být použité toopredict hello pravděpodobnost, že vstupní vektoru patří jednu skupinu nebo hello jiné.</span><span class="sxs-lookup"><span data-stu-id="57970-125">In summary, hello process of logistic regression produces a *logistic function* that can be used toopredict hello probability that an input vector belongs in one group or hello other.</span></span>  

## <a name="predictive-analysis-example-on-food-inspection-data"></a><span data-ttu-id="57970-126">Příklad prediktivní analýzy dat kontroly potravin</span><span class="sxs-lookup"><span data-stu-id="57970-126">Predictive analysis example on food inspection data</span></span>
<span data-ttu-id="57970-127">V tomto příkladu použijete Spark tooperform některé prediktivní analýzy dat kontroly potravin (**Food_Inspections1.csv**), byl získán v rámci hello [města Chicagu datovém portálu](https://data.cityofchicago.org/).</span><span class="sxs-lookup"><span data-stu-id="57970-127">In this example, you use Spark tooperform some predictive analysis on food inspection data (**Food_Inspections1.csv**) that was acquired through hello [City of Chicago data portal](https://data.cityofchicago.org/).</span></span> <span data-ttu-id="57970-128">Tato datová sada obsahuje informace o kontroly potravin zařízení, které byly provedeny v Chicagu, včetně informací o každé zařízení, porušení hello nalezen (pokud existuje) a hello výsledky kontroly hello.</span><span class="sxs-lookup"><span data-stu-id="57970-128">This dataset contains information about food establishment inspections that were conducted in Chicago, including information about each establishment, hello violations found (if any), and hello results of hello inspection.</span></span> <span data-ttu-id="57970-129">datový soubor CSV Hello je již k dispozici v účtu úložiště hello přidruženého k hello clusteru **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="57970-129">hello CSV data file is already available in hello storage account associated with hello cluster at **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span></span>

<span data-ttu-id="57970-130">V následujících kroků hello vývoji modelu toosee trvá toopass nebo selhání kontroly potravin.</span><span class="sxs-lookup"><span data-stu-id="57970-130">In hello steps below, you develop a model toosee what it takes toopass or fail a food inspection.</span></span>

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a><span data-ttu-id="57970-131">Začněte vytvářet Spark MMLib strojového učení aplikace</span><span class="sxs-lookup"><span data-stu-id="57970-131">Start building a Spark MMLib machine learning app</span></span>
1. <span data-ttu-id="57970-132">Z hello [portál Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="57970-132">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="57970-133">Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="57970-133">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
1. <span data-ttu-id="57970-134">Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="57970-134">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="57970-135">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="57970-135">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="57970-136">Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="57970-136">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="57970-137">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="57970-137">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. <span data-ttu-id="57970-138">Vytvořte poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="57970-138">Create a notebook.</span></span> <span data-ttu-id="57970-139">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="57970-139">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="57970-140">![Vytvoření poznámkového bloku Jupyter](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="57970-140">![Create a Jupyter notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "Create a new Jupyter notebook")</span></span>
1. <span data-ttu-id="57970-141">Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="57970-141">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="57970-142">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="57970-142">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="57970-143">![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "zadejte název pro hello Poznámkový blok")</span><span class="sxs-lookup"><span data-stu-id="57970-143">![Provide a name for hello notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Provide a name for hello notebook")</span></span>
1. <span data-ttu-id="57970-144">Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně.</span><span class="sxs-lookup"><span data-stu-id="57970-144">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="57970-145">kontexty Spark a Hive Hello jsou pro vás automaticky vytvoří při spuštění první buňky kódu hello.</span><span class="sxs-lookup"><span data-stu-id="57970-145">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="57970-146">Můžete začít vytvářet svůj počítač učení aplikace importováním hello typů nezbytných pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="57970-146">You can start building your machine learning application by importing hello types required for this scenario.</span></span> <span data-ttu-id="57970-147">toodo Ano, umístěte kurzor hello hello buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="57970-147">toodo so, place hello cursor in hello cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a><span data-ttu-id="57970-148">Vytvoření vstupní dataframe</span><span class="sxs-lookup"><span data-stu-id="57970-148">Construct an input dataframe</span></span>
<span data-ttu-id="57970-149">Můžeme použít `sqlContext` tooperform transformací strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="57970-149">We can use `sqlContext` tooperform transformations on structured data.</span></span> <span data-ttu-id="57970-150">první úlohou Hello je tooload hello ukázková data ((**Food_Inspections1.csv**)) do Spark SQL *dataframe*.</span><span class="sxs-lookup"><span data-stu-id="57970-150">hello first task is tooload hello sample data ((**Food_Inspections1.csv**)) into a Spark SQL *dataframe*.</span></span>

1. <span data-ttu-id="57970-151">Protože hello nezpracovaná data jsou ve formátu CSV, potřebujeme toouse hello Spark kontextu toopull každý jednotlivý řádek hello souboru do paměti jako nestrukturovaných text; potom použít jazyka Python sdíleného svazku clusteru knihovnu tooparse každý řádek zvlášť.</span><span class="sxs-lookup"><span data-stu-id="57970-151">Because hello raw data is in a CSV format, we need toouse hello Spark context toopull every line of hello file into memory as unstructured text; then, you use Python's CSV library tooparse each line individually.</span></span>

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. <span data-ttu-id="57970-152">Jako RDD máme teď hello souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="57970-152">We now have hello CSV file as an RDD.</span></span>  <span data-ttu-id="57970-153">schéma hello toounderstand hello dat, můžeme ze hello RDD načíst jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="57970-153">toounderstand hello schema of hello data, we retrieve one row from hello RDD.</span></span>

        inspections.take(1)

    <span data-ttu-id="57970-154">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-154">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of hello plumbing section of hello Municipal Code of Chicago and Rules and Regulation of hello Board of Health. OBSEVERD hello 3 COMPARTMENT SINK BACKING UP INTO hello 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding tooprotect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED tooREPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN hello REAR CHILDREN AREA,IN hello KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED tooREPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY hello 15MOS AREA. NEED tooBE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY hello EXPOSED HAND SINK IN hello KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: hello floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED tooELEVATE ALL FOOD ITEMS 6INCH OFF hello FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. <span data-ttu-id="57970-155">Hello předchozí výstup nám poskytuje základní přehled o hello schéma hello vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="57970-155">hello preceding output gives us an idea of hello schema of hello input file.</span></span> <span data-ttu-id="57970-156">Obsahuje název hello každé zařízení, hello typ zařízení, hello adresu, data hello hello kontrol a hello umístění, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="57970-156">It includes hello name of every establishment, hello type of establishment, hello address, hello data of hello inspections, and hello location, among other things.</span></span> <span data-ttu-id="57970-157">Umožňuje vybrat několik sloupců, které jsou užitečné pro naše prediktivní analýzy a výsledky hello skupiny jako dataframe, které jsme potom použít toocreate dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="57970-157">Let's select a few columns that are useful for our predictive analysis and group hello results as a dataframe, which we then use toocreate a temporary table.</span></span>

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. <span data-ttu-id="57970-158">Nyní je k dispozici *dataframe*, `df` na kterém jsme můžete provádět analýzy.</span><span class="sxs-lookup"><span data-stu-id="57970-158">We now have a *dataframe*, `df` on which we can perform our analysis.</span></span> <span data-ttu-id="57970-159">Máme také volání dočasné tabulky **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="57970-159">We also have a temporary table call **CountResults**.</span></span> <span data-ttu-id="57970-160">Jsme zahrnuli čtyři sloupce zájem o hello dataframe: **id**, **název**, **výsledky**, a **porušení**.</span><span class="sxs-lookup"><span data-stu-id="57970-160">We've included four columns of interest in hello dataframe: **id**, **name**, **results**, and **violations**.</span></span>

    <span data-ttu-id="57970-161">Pojďme malé ukázkové hello dat:</span><span class="sxs-lookup"><span data-stu-id="57970-161">Let's get a small sample of hello data:</span></span>

        df.show(5)

    <span data-ttu-id="57970-162">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-162">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES too...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-hello-data"></a><span data-ttu-id="57970-163">Pochopení hello dat</span><span class="sxs-lookup"><span data-stu-id="57970-163">Understand hello data</span></span>
1. <span data-ttu-id="57970-164">Začněme tooget představu o co obsahuje naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="57970-164">Let's start tooget a sense of what our dataset contains.</span></span> <span data-ttu-id="57970-165">Co jsou například hello různé hodnoty v hello **výsledky** sloupec?</span><span class="sxs-lookup"><span data-stu-id="57970-165">For example, what are hello different values in hello **results** column?</span></span>

        df.select('results').distinct().show()

    <span data-ttu-id="57970-166">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-166">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. <span data-ttu-id="57970-167">Rychlé vizualizace pomůžete nám důvod informace o distribuci hello tyto výstupy.</span><span class="sxs-lookup"><span data-stu-id="57970-167">A quick visualization can help us reason about hello distribution of these outcomes.</span></span> <span data-ttu-id="57970-168">Jsme již máte data, hello v dočasné tabulce **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="57970-168">We already have hello data in a temporary table **CountResults**.</span></span> <span data-ttu-id="57970-169">Můžete spustit následující dotaz SQL na hello tabulky tooget hello lépe porozumět rozdělení hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="57970-169">You can run hello following SQL query against hello table tooget a better understanding of how hello results are distributed.</span></span>

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    <span data-ttu-id="57970-170">Hello `%%sql` magic následuje `-o countResultsdf` zajistí, že výstup hello hello dotazu je trvalé místně na serveru Jupyter hello (obvykle hello headnode hello clusteru).</span><span class="sxs-lookup"><span data-stu-id="57970-170">hello `%%sql` magic followed by `-o countResultsdf` ensures that hello output of hello query is persisted locally on hello Jupyter server (typically hello headnode of hello cluster).</span></span> <span data-ttu-id="57970-171">výstup Hello je uchován jako [Pandas](http://pandas.pydata.org/) dataframe s hello zadaný název **countResultsdf**.</span><span class="sxs-lookup"><span data-stu-id="57970-171">hello output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with hello specified name **countResultsdf**.</span></span>

    <span data-ttu-id="57970-172">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-172">You should see an output like hello following:</span></span>

    <span data-ttu-id="57970-173">![Výstup dotazu SQL](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "výstupu dotazu SQL")</span><span class="sxs-lookup"><span data-stu-id="57970-173">![SQL query output](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL query output")</span></span>

    <span data-ttu-id="57970-174">Další informace o hello `%%sql` magic a dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="57970-174">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
1. <span data-ttu-id="57970-175">Můžete také Matplotlib, knihovny použita tooconstruct vizualizaci dat, toocreate vykreslení.</span><span class="sxs-lookup"><span data-stu-id="57970-175">You can also use Matplotlib, a library used tooconstruct visualization of data, toocreate a plot.</span></span> <span data-ttu-id="57970-176">Protože hello výkresu musí být vytvořeny z místně hello trvalé **countResultsdf** dataframe, fragment kódu hello musí začínat řetězcem hello `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="57970-176">Because hello plot must be created from hello locally persisted **countResultsdf** dataframe, hello code snippet must begin with hello `%%local` magic.</span></span> <span data-ttu-id="57970-177">To zajistí hello kód je místně na serveru Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="57970-177">This ensures that hello code is run locally on hello Jupyter server.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="57970-178">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-178">You should see an output like hello following:</span></span>

    <span data-ttu-id="57970-179">![Výstup Spark strojového učení aplikace - výsečového grafu s pěti výsledky kontroly odlišné](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark strojového učení výstup výsledků")</span><span class="sxs-lookup"><span data-stu-id="57970-179">![Spark machine learning application output - pie chart with five distinct inspection results](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning result output")</span></span>
1. <span data-ttu-id="57970-180">Můžete zjistit, že jsou 5 odlišné výsledky, které může mít kontrola:</span><span class="sxs-lookup"><span data-stu-id="57970-180">You can see that there are 5 distinct results that an inspection can have:</span></span>

   * <span data-ttu-id="57970-181">Obchodní není umístěný.</span><span class="sxs-lookup"><span data-stu-id="57970-181">Business not located</span></span>
   * <span data-ttu-id="57970-182">Selhání</span><span class="sxs-lookup"><span data-stu-id="57970-182">Fail</span></span>
   * <span data-ttu-id="57970-183">Předat</span><span class="sxs-lookup"><span data-stu-id="57970-183">Pass</span></span>
   * <span data-ttu-id="57970-184">PSS s podmínky</span><span class="sxs-lookup"><span data-stu-id="57970-184">Pss w/ conditions</span></span>
   * <span data-ttu-id="57970-185">Mimo firmy</span><span class="sxs-lookup"><span data-stu-id="57970-185">Out of Business</span></span>

     <span data-ttu-id="57970-186">Dejte nám vyvinout model, který lze snadno uhodnout hello výsledek kontroly potravin, porušení dané hello.</span><span class="sxs-lookup"><span data-stu-id="57970-186">Let us develop a model that can guess hello outcome of a food inspection, given hello violations.</span></span> <span data-ttu-id="57970-187">Vzhledem k tomu, že logistic regression je metoda binární klasifikace, má smysl toogroup naše data do dvou kategorií: **nezdaří** a **předat**.</span><span class="sxs-lookup"><span data-stu-id="57970-187">Since logistic regression is a binary classification method, it makes sense toogroup our data into two categories: **Fail** and **Pass**.</span></span> <span data-ttu-id="57970-188">"Předat s podmínky" je stále Pass, takže když jsme trénování modelu hello, jsme zvažte hello dva výsledků ekvivalent.</span><span class="sxs-lookup"><span data-stu-id="57970-188">A "Pass w/ Conditions" is still a Pass, so when we train hello model, we consider hello two results equivalent.</span></span> <span data-ttu-id="57970-189">Data s hello jiných výsledků ("Obchodní není umístěný" nebo "mimo firemní") nejsou užitečné proto jsme odebrat z našich trénovací sady.</span><span class="sxs-lookup"><span data-stu-id="57970-189">Data with hello other results ("Business Not Located" or "Out of Business") are not useful so we remove them from our training set.</span></span> <span data-ttu-id="57970-190">Toto musí být v pořádku, vzhledem k tomu, že tyto dvě kategorie tvoří velmi malá část výsledků hello přesto.</span><span class="sxs-lookup"><span data-stu-id="57970-190">This should be okay since these two categories make up a very small percentage of hello results anyway.</span></span>
1. <span data-ttu-id="57970-191">Dejte nám pokračujte a převést naše stávající dataframe (`df`) do nového dataframe, kde je každý kontroly reprezentován jako pár porušení popisek.</span><span class="sxs-lookup"><span data-stu-id="57970-191">Let us go ahead and convert our existing dataframe(`df`) into a new dataframe where each inspection is represented as a label-violations pair.</span></span> <span data-ttu-id="57970-192">V našem případě štítek z `0.0` představuje selhání, popisek `1.0` představuje úspěšné a popisek `-1.0` představuje některé výsledky kromě těchto dvou.</span><span class="sxs-lookup"><span data-stu-id="57970-192">In our case, a label of `0.0` represents a failure, a label of `1.0` represents a success, and a label of `-1.0` represents some results besides those two.</span></span> <span data-ttu-id="57970-193">Jsme odfiltrovat jiných výsledků při výpočtu hello nové datové rámce.</span><span class="sxs-lookup"><span data-stu-id="57970-193">We filter those other results out when computing hello new data frame.</span></span>

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    <span data-ttu-id="57970-194">toosee jaké hello označené data, která vypadá, můžeme načíst jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="57970-194">toosee what hello labeled data looks like, let's retrieve one row.</span></span>

        labeledData.take(1)

    <span data-ttu-id="57970-195">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-195">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of hello food establishment and all parts of hello property used in connection with hello operation of hello establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF hello FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: hello flow of air discharged from kitchen fans shall always be through a duct tooa point above hello roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT tooDINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT tooOFFICE.")]

## <a name="create-a-logistic-regression-model-from-hello-input-dataframe"></a><span data-ttu-id="57970-196">Vytvoření modelu logistic regression ze vstupní dataframe hello</span><span class="sxs-lookup"><span data-stu-id="57970-196">Create a logistic regression model from hello input dataframe</span></span>
<span data-ttu-id="57970-197">Naše poslední je označené data do formátu, který lze analyzovat pomocí logistic regression hello tooconvert.</span><span class="sxs-lookup"><span data-stu-id="57970-197">Our final task is tooconvert hello labeled data into a format that can be analyzed by logistic regression.</span></span> <span data-ttu-id="57970-198">Hello vstupní tooa logistic regression algoritmus musí být sadu *popisek funkce vector páry*, kde je hello "funkce vector" vektor čísel představující hello vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="57970-198">hello input tooa logistic regression algorithm should be a set of *label-feature vector pairs*, where hello "feature vector" is a vector of numbers representing hello input point.</span></span> <span data-ttu-id="57970-199">Ano potřebujeme tooconvert porušení"hello" sloupec, který je částečně strukturovaných a obsahuje mnoho komentáře ve volné, tooan pole reálná čísla, které by mohl snadno porozumíte na počítač.</span><span class="sxs-lookup"><span data-stu-id="57970-199">So, we need tooconvert hello "violations" column, which is semi-structured and contains many comments in free-text, tooan array of real numbers that a machine could easily understand.</span></span>

<span data-ttu-id="57970-200">Jeden standardní machine learning přístup pro zpracování přirozeného jazyka je tooassign všech jedinečných slov "index" a pak předejte vektoru toohello algoritmus strojového učení tak, aby každý index hodnota obsahuje hello relativní frekvenci dané slovo v textu hello řetězec.</span><span class="sxs-lookup"><span data-stu-id="57970-200">One standard machine learning approach for processing natural language is tooassign each distinct word an "index", and then pass a vector toohello machine learning algorithm such that each index's value contains hello relative frequency of that word in hello text string.</span></span>

<span data-ttu-id="57970-201">MLlib poskytuje snadný způsob tooperform tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="57970-201">MLlib provides an easy way tooperform this operation.</span></span> <span data-ttu-id="57970-202">Nejprve "tokenizaci" každé porušení řetězec tooget hello jednotlivých slov v každé řetězci.</span><span class="sxs-lookup"><span data-stu-id="57970-202">First, "tokenize" each violations string tooget hello individual words in each string.</span></span> <span data-ttu-id="57970-203">Poté použijte `HashingTF` tooconvert každou sadu tokeny do funkce vektor, který lze potom předaný toohello logistic regression algoritmus tooconstruct modelu.</span><span class="sxs-lookup"><span data-stu-id="57970-203">Then, use a `HashingTF` tooconvert each set of tokens into a feature vector that can then be passed toohello logistic regression algorithm tooconstruct a model.</span></span> <span data-ttu-id="57970-204">Můžeme provést všechny tyto kroky v pořadí pomocí "kanál".</span><span class="sxs-lookup"><span data-stu-id="57970-204">We conduct all of these steps in sequence using a "pipeline".</span></span>

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-hello-model-on-a-separate-test-dataset"></a><span data-ttu-id="57970-205">Vyhodnocení modelu hello na samostatné testovací datové sady</span><span class="sxs-lookup"><span data-stu-id="57970-205">Evaluate hello model on a separate test dataset</span></span>
<span data-ttu-id="57970-206">Můžeme použít model hello jsme vytvořili předtím příliš*předpovědi* jaké hello výsledky kontrol nové budou, podle hello porušení zásad, která měla dodržen.</span><span class="sxs-lookup"><span data-stu-id="57970-206">We can use hello model we created earlier too*predict* what hello results of new inspections will be, based on hello violations that were observed.</span></span> <span data-ttu-id="57970-207">Jsme natrénovali tento model pro datovou sadu hello **Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="57970-207">We trained this model on hello dataset **Food_Inspections1.csv**.</span></span> <span data-ttu-id="57970-208">Dejte nám použít druhý datovou sadu, **Food_Inspections2.csv**, příliš*vyhodnotit* hello sílu tento model na nová data.</span><span class="sxs-lookup"><span data-stu-id="57970-208">Let us use a second dataset, **Food_Inspections2.csv**, too*evaluate* hello strength of this model on new data.</span></span> <span data-ttu-id="57970-209">Druhé sadě dat (**Food_Inspections2.csv**) by už měla být v hello výchozí kontejner úložiště přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="57970-209">This second data set (**Food_Inspections2.csv**) should already be in hello default storage container associated with hello cluster.</span></span>

1. <span data-ttu-id="57970-210">Hello následující fragment vytváří nové dataframe, **predictionsDf** obsahující hello předpovědi generované hello modelu.</span><span class="sxs-lookup"><span data-stu-id="57970-210">hello following snippet creates a new dataframe, **predictionsDf** that contains hello prediction generated by hello model.</span></span> <span data-ttu-id="57970-211">fragment kódu Hello také vytvoří dočasné tabulky nazývané **předpovědi** podle hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="57970-211">hello snippet also creates a temporary table called **Predictions** based on hello dataframe.</span></span>

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    <span data-ttu-id="57970-212">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-212">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. <span data-ttu-id="57970-213">Podívejte se na jednu z hello předpovědi.</span><span class="sxs-lookup"><span data-stu-id="57970-213">Look at one of hello predictions.</span></span> <span data-ttu-id="57970-214">Spusťte tento fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="57970-214">Run this snippet:</span></span>

        predictionsDf.take(1)

   <span data-ttu-id="57970-215">Není předpovědi hello první položku v hello testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="57970-215">There is a prediction for hello first entry in hello test data set.</span></span>
1. <span data-ttu-id="57970-216">Hello `model.transform()` metoda platí hello stejnou transformaci tooany nová data s hello stejné schéma a přicházejí na předpověď jak tooclassify hello data.</span><span class="sxs-lookup"><span data-stu-id="57970-216">hello `model.transform()` method applies hello same transformation tooany new data with hello same schema, and arrive at a prediction of how tooclassify hello data.</span></span> <span data-ttu-id="57970-217">Některé jednoduché statistiky tooget můžeme představu o jak přesný byly naše předpovědi udělat:</span><span class="sxs-lookup"><span data-stu-id="57970-217">We can do some simple statistics tooget a sense of how accurate our predictions were:</span></span>

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    <span data-ttu-id="57970-218">výstup Hello vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="57970-218">hello output looks like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    <span data-ttu-id="57970-219">Pomocí Spark logistic regression nám poskytuje model přesné hello vztahu mezi popisy porušení v angličtině a zda se daný obchodní úspěch nebo selhání kontroly potravin.</span><span class="sxs-lookup"><span data-stu-id="57970-219">Using logistic regression with Spark gives us an accurate model of hello relationship between violations descriptions in English and whether a given business would pass or fail a food inspection.</span></span>

## <a name="create-a-visual-representation-of-hello-prediction"></a><span data-ttu-id="57970-220">Vytvoření vizuální reprezentace předpovědi hello</span><span class="sxs-lookup"><span data-stu-id="57970-220">Create a visual representation of hello prediction</span></span>
<span data-ttu-id="57970-221">Nyní jsme můžete vytvořit toohelp konečné vizualizace, které nám důvodu o hello výsledky tento test.</span><span class="sxs-lookup"><span data-stu-id="57970-221">We can now construct a final visualization toohelp us reason about hello results of this test.</span></span>

1. <span data-ttu-id="57970-222">Začneme extrahováním hello různých předpovědi a výsledky z hello **předpovědi** dočasné tabulky vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="57970-222">We start by extracting hello different predictions and results from hello **Predictions** temporary table created earlier.</span></span> <span data-ttu-id="57970-223">Hello následující dotazy oddělit výstup hello jako *true_positive*, *false_positive*, *true_negative*, a *false_negative*.</span><span class="sxs-lookup"><span data-stu-id="57970-223">hello following queries separate hello output as *true_positive*, *false_positive*, *true_negative*, and *false_negative*.</span></span> <span data-ttu-id="57970-224">V dotazech hello níže, jsme vypnout vizualizaci pomocí `-q` a také uložit výstup hello (pomocí `-o`) jako dataframes, který lze potom použít s hello `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="57970-224">In hello queries below, we turn off visualization by using `-q` and also save hello output (by using `-o`) as dataframes that can be then used with hello `%%local` magic.</span></span>

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. <span data-ttu-id="57970-225">Nakonec použijte následující fragment kódu toogenerate hello výkresu pomocí hello **Matplotlib**.</span><span class="sxs-lookup"><span data-stu-id="57970-225">Finally, use hello following snippet toogenerate hello plot using **Matplotlib**.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="57970-226">Měli byste vidět hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="57970-226">You should see hello following output:</span></span>

    <span data-ttu-id="57970-227">![Spark strojového učení výstupu aplikace - výsečového grafu procenta kontroly potravin se nezdařilo. ] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark strojového učení výstup výsledků")</span><span class="sxs-lookup"><span data-stu-id="57970-227">![Spark machine learning application output - pie chart percentages of failed food inspections.](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning result output")</span></span>

    <span data-ttu-id="57970-228">V tomto grafu odkazuje "pozitivní" výsledek kontroly potravin toohello se nezdařilo, zatímco záporný výsledek odkazuje tooa předán kontroly.</span><span class="sxs-lookup"><span data-stu-id="57970-228">In this chart, a "positive" result refers toohello failed food inspection, while a negative result refers tooa passed inspection.</span></span>

## <a name="shut-down-hello-notebook"></a><span data-ttu-id="57970-229">Vypnout hello Poznámkový blok</span><span class="sxs-lookup"><span data-stu-id="57970-229">Shut down hello notebook</span></span>
<span data-ttu-id="57970-230">Po dokončení spuštění aplikace hello byste měli vypínat hello poznámkového bloku toorelease hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="57970-230">After you have finished running hello application, you should shut down hello notebook toorelease hello resources.</span></span> <span data-ttu-id="57970-231">toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.</span><span class="sxs-lookup"><span data-stu-id="57970-231">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="57970-232">To ukončí a zavře hello poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="57970-232">This shuts down and closes hello notebook.</span></span>

## <span data-ttu-id="57970-233"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="57970-233"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="57970-234">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="57970-234">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="57970-235">Scénáře</span><span class="sxs-lookup"><span data-stu-id="57970-235">Scenarios</span></span>
* [<span data-ttu-id="57970-236">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="57970-236">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="57970-237">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="57970-237">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="57970-238">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="57970-238">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="57970-239">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57970-239">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="57970-240">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="57970-240">Create and run applications</span></span>
* [<span data-ttu-id="57970-241">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="57970-241">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="57970-242">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="57970-242">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="57970-243">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="57970-243">Tools and extensions</span></span>
* [<span data-ttu-id="57970-244">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="57970-244">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="57970-245">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="57970-245">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="57970-246">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57970-246">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="57970-247">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="57970-247">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="57970-248">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="57970-248">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="57970-249">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="57970-249">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="57970-250">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="57970-250">Manage resources</span></span>
* [<span data-ttu-id="57970-251">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="57970-251">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="57970-252">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57970-252">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
