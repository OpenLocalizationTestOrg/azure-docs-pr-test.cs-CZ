---
title: "Vědecké zpracování dat pomocí Scala a Spark v Azure | Microsoft Docs"
description: "Jak používat Scala pro úkoly pod dohledem strojového učení s Spark škálovatelné MLlib a Spark ML balíčky v clusteru Azure HDInsight Spark."
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
ms.openlocfilehash: b2419f53bdc3236d7de76b89f2a0a76704e85391
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="data-science-using-scala-and-spark-on-azure"></a><span data-ttu-id="ff05e-103">Vědecké zkoumání dat pomocí Scala a Spark v Azure</span><span class="sxs-lookup"><span data-stu-id="ff05e-103">Data Science using Scala and Spark on Azure</span></span>
<span data-ttu-id="ff05e-104">Tento článek ukazuje, jak používat Scala pro úkoly strojového učení s Spark škálovatelné MLlib a Spark ML balíčky v clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-104">This article shows you how to use Scala for supervised machine learning tasks with the Spark scalable MLlib and Spark ML packages on an Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="ff05e-105">Provede vás provedou úlohami, které tvoří [vědecké zpracování dat proces](http://aka.ms/datascienceprocess): přijímání dat a zkoumání, vizualizace, funkce analýzy, modelování a spotřeba modelu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-105">It walks you through the tasks that constitute the [Data Science process](http://aka.ms/datascienceprocess): data ingestion and exploration, visualization, feature engineering, modeling, and model consumption.</span></span> <span data-ttu-id="ff05e-106">Modely v článku zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromy (GBTs), kromě dvě běžné úkoly strojového učení:</span><span class="sxs-lookup"><span data-stu-id="ff05e-106">The models in the article include logistic and linear regression, random forests, and gradient-boosted trees (GBTs), in addition to two common supervised machine learning tasks:</span></span>

* <span data-ttu-id="ff05e-107">Regrese problému: předpovědi velikost tip ($) pro cestu taxíkem</span><span class="sxs-lookup"><span data-stu-id="ff05e-107">Regression problem: Prediction of the tip amount ($) for a taxi trip</span></span>
* <span data-ttu-id="ff05e-108">Binární klasifikace: předpovědi tip nebo cesty taxíkem žádné tip (1 nebo 0)</span><span class="sxs-lookup"><span data-stu-id="ff05e-108">Binary classification: Prediction of tip or no tip (1/0) for a taxi trip</span></span>

<span data-ttu-id="ff05e-109">Proces modelování vyžaduje trénování a hodnocení na testovací datové sady a relevantní přesnost metriky.</span><span class="sxs-lookup"><span data-stu-id="ff05e-109">The modeling process requires training and evaluation on a test data set and relevant accuracy metrics.</span></span> <span data-ttu-id="ff05e-110">V tomto článku můžete zjistěte, jak ukládat tyto modely v Azure Blob storage a jak stanovení skóre a vyhodnotit jejich prediktivní výkonu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-110">In this article, you can learn how to store these models in Azure Blob storage and how to score and evaluate their predictive performance.</span></span> <span data-ttu-id="ff05e-111">Tento článek se týká také pokročilejším tématům o tom, jak optimalizovat modely s použitím sweeping křížové ověření a technologie hyper parametr.</span><span class="sxs-lookup"><span data-stu-id="ff05e-111">This article also covers the more advanced topics of how to optimize models by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="ff05e-112">Data použitá je ukázka 2013 NYC taxíkem služební cestě a tarif datová sada k dispozici na Githubu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-112">The data used is a sample of the 2013 NYC taxi trip and fare data set available on GitHub.</span></span>

<span data-ttu-id="ff05e-113">[Scala](http://www.scala-lang.org/), jazyk založené na virtuálním počítači Java integruje koncepty jazyka objektově orientované a funkční.</span><span class="sxs-lookup"><span data-stu-id="ff05e-113">[Scala](http://www.scala-lang.org/), a language based on the Java virtual machine, integrates object-oriented and functional language concepts.</span></span> <span data-ttu-id="ff05e-114">Je škálovatelná jazyk, který skvěle hodí pro distribuované zpracování v cloudu a běží na clustery Spark v Azure.</span><span class="sxs-lookup"><span data-stu-id="ff05e-114">It's a scalable language that is well suited to distributed processing in the cloud, and runs on Azure Spark clusters.</span></span>

<span data-ttu-id="ff05e-115">[Spark](http://spark.apache.org/) představuje rozhraní pro paralelní zpracování open source, který podporuje zpracování v paměti pro zvýšení výkonu aplikací analýzy velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="ff05e-115">[Spark](http://spark.apache.org/) is an open-source parallel-processing framework that supports in-memory processing to boost the performance of big data analytics applications.</span></span> <span data-ttu-id="ff05e-116">Modul zpracování Spark je vytvořené pro rychlost, snadné použití a sofistikované analytics.</span><span class="sxs-lookup"><span data-stu-id="ff05e-116">The Spark processing engine is built for speed, ease of use, and sophisticated analytics.</span></span> <span data-ttu-id="ff05e-117">Možnosti v paměti distribuované výpočtů Spark ho nastavit správnou volbu pro iterativní algoritmy v machine learning a grafů výpočty.</span><span class="sxs-lookup"><span data-stu-id="ff05e-117">Spark's in-memory distributed computation capabilities make it a good choice for iterative algorithms in machine learning and graph computations.</span></span> <span data-ttu-id="ff05e-118">[Spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) balíček poskytuje jednotnou sadu vysoké úrovně rozhraní API vytvořená na základě dat snímků, které vám pomůžou vytvářet a ladit praktické strojového učení kanály.</span><span class="sxs-lookup"><span data-stu-id="ff05e-118">The [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) package provides a uniform set of high-level APIs built on top of data frames that can help you create and tune practical machine learning pipelines.</span></span> <span data-ttu-id="ff05e-119">[MLlib](http://spark.apache.org/mllib/) je Spark škálovatelné machine learning knihovny, která přináší modelování možnosti pro tento distribuovaném prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff05e-119">[MLlib](http://spark.apache.org/mllib/) is Spark's scalable machine learning library, which brings modeling capabilities to this distributed environment.</span></span>

<span data-ttu-id="ff05e-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) je nabídku Azure hostovaná Spark open source.</span><span class="sxs-lookup"><span data-stu-id="ff05e-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) is the Azure-hosted offering of open-source Spark.</span></span> <span data-ttu-id="ff05e-121">Také zahrnuje podporu pro poznámkové bloky Jupyter Scala na clusteru Spark a můžete spustit interaktivních dotazů Spark SQL k transformaci, filtrování a vizualizovat data uložená v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="ff05e-121">It also includes support for Jupyter Scala notebooks on the Spark cluster, and can run Spark SQL interactive queries to transform, filter, and visualize data stored in Azure Blob storage.</span></span> <span data-ttu-id="ff05e-122">Fragmenty kódu Scala v tomto článku s řešení, které ukazují relevantní pozemků k vizualizaci dat spustit v poznámkové bloky Jupyter nainstalovat na clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-122">The Scala code snippets in this article that provide the solutions and show the relevant plots to visualize the data run in Jupyter notebooks installed on the Spark clusters.</span></span> <span data-ttu-id="ff05e-123">Modelování kroky v těchto tématech obsahovat kód, který ukazuje, jak cvičení, hodnocení, uložit a používat každý typ modelu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-123">The modeling steps in these topics have code that shows you how to train, evaluate, save, and consume each type of model.</span></span>

<span data-ttu-id="ff05e-124">Postup instalace a kódu v tomto článku jsou pro Azure HDInsight 3.4 Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="ff05e-124">The setup steps and code in this article are for Azure HDInsight 3.4 Spark 1.6.</span></span> <span data-ttu-id="ff05e-125">Ale kód v tomto článku a v [poznámkového bloku Jupyter Scala](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) obecné a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-125">However, the code in this article and in the [Scala Jupyter Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) are generic and should work on any Spark cluster.</span></span> <span data-ttu-id="ff05e-126">Kroky instalace a Správa clusteru může být mírně lišit od co se zobrazí v tomto článku, pokud nepoužíváte HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-126">The cluster setup and management steps might be slightly different from what is shown in this article if you are not using HDInsight Spark.</span></span>

> [!NOTE]
> <span data-ttu-id="ff05e-127">Téma, které ukazuje, jak k dokončení úloh na začátku do konce proces vědecké zpracování dat použít Python, nikoli Scala, najdete v části [vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff05e-127">For a topic that shows you how to use Python rather than Scala to complete tasks for an end-to-end Data Science process, see [Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ff05e-128">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff05e-128">Prerequisites</span></span>
* <span data-ttu-id="ff05e-129">Musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ff05e-129">You must have an Azure subscription.</span></span> <span data-ttu-id="ff05e-130">Pokud jste již nemají, [získat bezplatnou zkušební verzi Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ff05e-130">If you do not already have one, [get an Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="ff05e-131">Budete potřebovat clusteru Azure HDInsight 3.4 Spark 1.6 proveďte následující postupy.</span><span class="sxs-lookup"><span data-stu-id="ff05e-131">You need an Azure HDInsight 3.4 Spark 1.6 cluster to complete the following procedures.</span></span> <span data-ttu-id="ff05e-132">K vytvoření clusteru, postupujte podle pokynů v [Začínáme: Vytvořte Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ff05e-132">To create a cluster, see the instructions in [Get started: Create Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="ff05e-133">Nastavení clusteru typ a verze na **vybrat typ clusteru** nabídky.</span><span class="sxs-lookup"><span data-stu-id="ff05e-133">Set the cluster type and version on the **Select Cluster Type** menu.</span></span>

![Konfigurace typu clusteru HDInsight](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

<span data-ttu-id="ff05e-135">Popis NYC taxíkem cestě dat a pokyny, jak provést kód z poznámkového bloku Jupyter v clusteru Spark, najdete v příslušných částech [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff05e-135">For a description of the NYC taxi trip data and instructions on how to execute code from a Jupyter notebook on the Spark cluster, see the relevant sections in [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a><span data-ttu-id="ff05e-136">Spuštění z poznámkového bloku Jupyter v clusteru Spark Scala kódu</span><span class="sxs-lookup"><span data-stu-id="ff05e-136">Execute Scala code from a Jupyter notebook on the Spark cluster</span></span>
<span data-ttu-id="ff05e-137">Můžete spustit poznámkového bloku Jupyter z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ff05e-137">You can launch a Jupyter notebook from the Azure portal.</span></span> <span data-ttu-id="ff05e-138">Najděte cluster Spark na řídicím panelu a klikněte na něj zadejte na stránce Správa clusteru.</span><span class="sxs-lookup"><span data-stu-id="ff05e-138">Find the Spark cluster on your dashboard, and then click it to enter the management page for your cluster.</span></span> <span data-ttu-id="ff05e-139">Klikněte na tlačítko **řídicí panely clusteru**a potom klikněte na **Poznámkový blok Jupyter** otevřete Poznámkový blok spojené s clusterem Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-139">Next, click **Cluster Dashboards**, and then click **Jupyter Notebook** to open the notebook associated with the Spark cluster.</span></span>

![Řídicí panel clusteru a poznámkové bloky Jupyter](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

<span data-ttu-id="ff05e-141">Také můžete přístup poznámkové bloky Jupyter na https://&lt;clustername&gt;.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="ff05e-141">You also can access Jupyter notebooks at https://&lt;clustername&gt;.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="ff05e-142">Nahraďte *clustername* s názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="ff05e-142">Replace *clustername* with the name of your cluster.</span></span> <span data-ttu-id="ff05e-143">Potřebujete heslo pro účet správce pro přístup k Jupyter notebooks.</span><span class="sxs-lookup"><span data-stu-id="ff05e-143">You need the password for your administrator account to access the Jupyter notebooks.</span></span>

![Přejděte na poznámkové bloky Jupyter s použitím názvu clusteru](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

<span data-ttu-id="ff05e-145">Vyberte **Scala** zobrazíte adresář, který má několik příkladů hotových poznámkových bloků, které používají rozhraní API PySpark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-145">Select **Scala** to see a directory that has a few examples of prepackaged notebooks that use the PySpark API.</span></span> <span data-ttu-id="ff05e-146">Ukázky zkoumání modelování a vyhodnocování pomocí poznámkového bloku Scala.ipynb, který obsahuje kód pro tuto sadu témat Spark je k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span><span class="sxs-lookup"><span data-stu-id="ff05e-146">The Exploration Modeling and Scoring using Scala.ipynb notebook that contains the code samples for this suite of Spark topics is available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span></span>

<span data-ttu-id="ff05e-147">Můžete nahrát poznámkového bloku přímo z Githubu se serverem Poznámkový blok Jupyter v clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-147">You can upload the notebook directly from GitHub to the Jupyter Notebook server on your Spark cluster.</span></span> <span data-ttu-id="ff05e-148">Na domovské stránce Jupyter, klikněte **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff05e-148">On your Jupyter home page, click the **Upload** button.</span></span> <span data-ttu-id="ff05e-149">V Průzkumníku souborů, vložte adresu URL GitHub (nezpracovaná obsah) Scala Poznámkový blok a pak klikněte na **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="ff05e-149">In the file explorer, paste the GitHub (raw content) URL of the Scala notebook, and then click **Open**.</span></span> <span data-ttu-id="ff05e-150">Poznámkový blok Scala je k dispozici na následující adrese URL:</span><span class="sxs-lookup"><span data-stu-id="ff05e-150">The Scala notebook is available at the following URL:</span></span>

[<span data-ttu-id="ff05e-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span><span class="sxs-lookup"><span data-stu-id="ff05e-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span></span>](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a><span data-ttu-id="ff05e-152">Instalace: Kontexty přednastavených Spark a Hive, Spark Magic a knihoven Spark</span><span class="sxs-lookup"><span data-stu-id="ff05e-152">Setup: Preset Spark and Hive contexts, Spark magics, and Spark libraries</span></span>
### <a name="preset-spark-and-hive-contexts"></a><span data-ttu-id="ff05e-153">Předvolby kontexty Spark a Hive</span><span class="sxs-lookup"><span data-stu-id="ff05e-153">Preset Spark and Hive contexts</span></span>
    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


<span data-ttu-id="ff05e-154">Kontexty mít přednastavení jádrech Spark, které jsou k dispozici s poznámkovými bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="ff05e-154">The Spark kernels that are provided with Jupyter notebooks have preset contexts.</span></span> <span data-ttu-id="ff05e-155">Nemusíte explicitně nastavit Spark nebo vývoji Hive kontexty před zahájením práce s aplikací.</span><span class="sxs-lookup"><span data-stu-id="ff05e-155">You don't need to explicitly set the Spark or Hive contexts before you start working with the application you are developing.</span></span> <span data-ttu-id="ff05e-156">Přednastavené kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="ff05e-156">The preset contexts are:</span></span>

* <span data-ttu-id="ff05e-157">`sc`pro SparkContext</span><span class="sxs-lookup"><span data-stu-id="ff05e-157">`sc` for SparkContext</span></span>
* <span data-ttu-id="ff05e-158">`sqlContext`pro HiveContext</span><span class="sxs-lookup"><span data-stu-id="ff05e-158">`sqlContext` for HiveContext</span></span>

### <a name="spark-magics"></a><span data-ttu-id="ff05e-159">Spark Magic</span><span class="sxs-lookup"><span data-stu-id="ff05e-159">Spark magics</span></span>
<span data-ttu-id="ff05e-160">Spark jádra poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s `%%`.</span><span class="sxs-lookup"><span data-stu-id="ff05e-160">The Spark kernel provides some predefined “magics,” which are special commands that you can call with `%%`.</span></span> <span data-ttu-id="ff05e-161">V následující ukázky kódu se používají dva z těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="ff05e-161">Two of these commands are used in the following code samples.</span></span>

* <span data-ttu-id="ff05e-162">`%%local`Určuje, že kód na další řádek bude proveden místně.</span><span class="sxs-lookup"><span data-stu-id="ff05e-162">`%%local` specifies that the code in subsequent lines will be executed locally.</span></span> <span data-ttu-id="ff05e-163">Kód musí být platný kód Scala.</span><span class="sxs-lookup"><span data-stu-id="ff05e-163">The code must be valid Scala code.</span></span>
* <span data-ttu-id="ff05e-164">`%%sql -o <variable name>`provede dotaz Hive proti `sqlContext`.</span><span class="sxs-lookup"><span data-stu-id="ff05e-164">`%%sql -o <variable name>` executes a Hive query against `sqlContext`.</span></span> <span data-ttu-id="ff05e-165">Pokud `-o` parametr se předává, výsledek dotazu je uchován v `%%local` Scala kontextu jako snímek dat Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-165">If the `-o` parameter is passed, the result of the query is persisted in the `%%local` Scala context as a Spark data frame.</span></span>

<span data-ttu-id="ff05e-166">Pro další informace o jádrech pro poznámkové bloky Jupyter a jejich předdefinované "magics", volání s `%%` (například `%%local`), najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="ff05e-166">For more information about the kernels for Jupyter notebooks and their predefined "magics" that you call with `%%` (for example, `%%local`), see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

### <a name="import-libraries"></a><span data-ttu-id="ff05e-167">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="ff05e-167">Import libraries</span></span>
<span data-ttu-id="ff05e-168">Importujte Spark, MLlib a další knihovny, které budete potřebovat pomocí následující kód.</span><span class="sxs-lookup"><span data-stu-id="ff05e-168">Import the Spark, MLlib, and other libraries you'll need by using the following code.</span></span>

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


## <a name="data-ingestion"></a><span data-ttu-id="ff05e-169">Přijímání dat</span><span class="sxs-lookup"><span data-stu-id="ff05e-169">Data ingestion</span></span>
<span data-ttu-id="ff05e-170">Prvním krokem v procesu vědecké zpracování dat je ingestovat data, která chcete analyzovat.</span><span class="sxs-lookup"><span data-stu-id="ff05e-170">The first step in the Data Science process is to ingest the data that you want to analyze.</span></span> <span data-ttu-id="ff05e-171">Přenést data z externích zdrojů nebo systémy, kde se nachází do vašeho prostředí zkoumání a modelování data.</span><span class="sxs-lookup"><span data-stu-id="ff05e-171">You bring the data from external sources or systems where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="ff05e-172">V tomto článku data, která jste ingestování je připojený k ukázka 0,1 % taxíkem služební cestě a tarif souboru (uložený jako soubor TSV).</span><span class="sxs-lookup"><span data-stu-id="ff05e-172">In this article, the data you ingest is a joined 0.1% sample of the taxi trip and fare file (stored as a .tsv file).</span></span> <span data-ttu-id="ff05e-173">Data zkoumání a modelování prostředí je Spark.</span><span class="sxs-lookup"><span data-stu-id="ff05e-173">The data exploration and modeling environment is Spark.</span></span> <span data-ttu-id="ff05e-174">Tato část obsahuje kód pro dokončení následující řadu úloh:</span><span class="sxs-lookup"><span data-stu-id="ff05e-174">This section contains the code to complete the following series of tasks:</span></span>

1. <span data-ttu-id="ff05e-175">Nastavit cesty adresáře pro data a modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ff05e-175">Set directory paths for data and model storage.</span></span>
2. <span data-ttu-id="ff05e-176">Přečtěte si v sadě vstupní data (uložené jako soubor TSV).</span><span class="sxs-lookup"><span data-stu-id="ff05e-176">Read in the input data set (stored as a .tsv file).</span></span>
3. <span data-ttu-id="ff05e-177">Definovat schéma pro data a vyčistit data.</span><span class="sxs-lookup"><span data-stu-id="ff05e-177">Define a schema for the data and clean the data.</span></span>
4. <span data-ttu-id="ff05e-178">Vytvořte rámeček vyčištěnými dat a uložení do mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="ff05e-178">Create a cleaned data frame and cache it in memory.</span></span>
5. <span data-ttu-id="ff05e-179">Zaregistrujte se data jako do dočasné tabulky v SQLContext.</span><span class="sxs-lookup"><span data-stu-id="ff05e-179">Register the data as a temporary table in SQLContext.</span></span>
6. <span data-ttu-id="ff05e-180">Dotaz tabulku a importovat výsledky do rámečku data.</span><span class="sxs-lookup"><span data-stu-id="ff05e-180">Query the table and import the results into a data frame.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a><span data-ttu-id="ff05e-181">Nastavení cesty adresáře pro umístění úložiště v Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="ff05e-181">Set directory paths for storage locations in Azure Blob storage</span></span>
<span data-ttu-id="ff05e-182">Spark můžete číst a zapisovat do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ff05e-182">Spark can read and write to Azure Blob storage.</span></span> <span data-ttu-id="ff05e-183">Můžete používat Spark ke zpracování stávající data a pak znovu ukládání výsledků do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="ff05e-183">You can use Spark to process any of your existing data, and then store the results again in Blob storage.</span></span>

<span data-ttu-id="ff05e-184">Chcete-li uložit modely nebo soubory v úložišti objektů Blob, správně zadejte cestu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-184">To save models or files in Blob storage, you need to properly specify the path.</span></span> <span data-ttu-id="ff05e-185">Referenční výchozí kontejner, který je připojen ke clusteru Spark pomocí cesty, který začíná `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="ff05e-185">Reference the default container attached to the Spark cluster by using a path that begins with `wasb:///`.</span></span> <span data-ttu-id="ff05e-186">Referenční jiných umístění pomocí `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="ff05e-186">Reference other locations by using `wasb://`.</span></span>

<span data-ttu-id="ff05e-187">Následující ukázka kódu určuje umístění jsou vstupní data čtení a cestu k úložišti objektů Blob, který je připojen ke clusteru Spark, kde bude uložen modelu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-187">The following code sample specifies the location of the input data to be read and the path to Blob storage that is attached to the Spark cluster where the model will be saved.</span></span>

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a><span data-ttu-id="ff05e-188">Umožňuje importovat data, vytvořit RDD a definovat data rámce podle schématu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-188">Import data, create an RDD, and define a data frame according to the schema</span></span>
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
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

    # CAST VARIABLES ACCORDING TO THE SCHEMA
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

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="ff05e-189">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-189">**Output:**</span></span>

<span data-ttu-id="ff05e-190">Čas spuštění buňky: 8 sekund.</span><span class="sxs-lookup"><span data-stu-id="ff05e-190">Time to run the cell: 8 seconds.</span></span>

### <a name="query-the-table-and-import-results-in-a-data-frame"></a><span data-ttu-id="ff05e-191">Dotaz tabulku a importovat výsledky v rámci dat</span><span class="sxs-lookup"><span data-stu-id="ff05e-191">Query the table and import results in a data frame</span></span>
<span data-ttu-id="ff05e-192">V dalším kroku dotazu v tabulce tarif, osobní a tip data; filtrování dat poškozen a odlehlé; a tisk několik řádků.</span><span class="sxs-lookup"><span data-stu-id="ff05e-192">Next, query the table for fare, passenger, and tip data; filter out corrupt and outlying data; and print several rows.</span></span>

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

<span data-ttu-id="ff05e-193">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-193">**Output:**</span></span>

| <span data-ttu-id="ff05e-194">fare_amount</span><span class="sxs-lookup"><span data-stu-id="ff05e-194">fare_amount</span></span> | <span data-ttu-id="ff05e-195">passenger_count</span><span class="sxs-lookup"><span data-stu-id="ff05e-195">passenger_count</span></span> | <span data-ttu-id="ff05e-196">tip_amount</span><span class="sxs-lookup"><span data-stu-id="ff05e-196">tip_amount</span></span> | <span data-ttu-id="ff05e-197">vysypávány</span><span class="sxs-lookup"><span data-stu-id="ff05e-197">tipped</span></span> |
| --- | --- | --- | --- |
|        <span data-ttu-id="ff05e-198">13.5</span><span class="sxs-lookup"><span data-stu-id="ff05e-198">13.5</span></span> |<span data-ttu-id="ff05e-199">1.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-199">1.0</span></span> |<span data-ttu-id="ff05e-200">2.9</span><span class="sxs-lookup"><span data-stu-id="ff05e-200">2.9</span></span> |<span data-ttu-id="ff05e-201">1.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-201">1.0</span></span> |
|        <span data-ttu-id="ff05e-202">16.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-202">16.0</span></span> |<span data-ttu-id="ff05e-203">2.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-203">2.0</span></span> |<span data-ttu-id="ff05e-204">3.4</span><span class="sxs-lookup"><span data-stu-id="ff05e-204">3.4</span></span> |<span data-ttu-id="ff05e-205">1.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-205">1.0</span></span> |
|        <span data-ttu-id="ff05e-206">10.5</span><span class="sxs-lookup"><span data-stu-id="ff05e-206">10.5</span></span> |<span data-ttu-id="ff05e-207">2.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-207">2.0</span></span> |<span data-ttu-id="ff05e-208">1.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-208">1.0</span></span> |<span data-ttu-id="ff05e-209">1.0</span><span class="sxs-lookup"><span data-stu-id="ff05e-209">1.0</span></span> |

## <a name="data-exploration-and-visualization"></a><span data-ttu-id="ff05e-210">Zkoumání dat a vizualizaci</span><span class="sxs-lookup"><span data-stu-id="ff05e-210">Data exploration and visualization</span></span>
<span data-ttu-id="ff05e-211">Po přenést data do Spark, je dalším krokem v procesu vědecké zpracování dat získali lepší představu o dat přes zkoumání a vizualizace.</span><span class="sxs-lookup"><span data-stu-id="ff05e-211">After you bring the data into Spark, the next step in the Data Science process is to gain a deeper understanding of the data through exploration and visualization.</span></span> <span data-ttu-id="ff05e-212">V této části Zkontrolujte taxíkem dat pomocí dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="ff05e-212">In this section, you examine the taxi data by using SQL queries.</span></span> <span data-ttu-id="ff05e-213">Pak importujte výsledky do rámečku dat k vykreslení cílových proměnných a potenciální funkcí pro visual kontroly pomocí funkce Automatické vizualizace z Jupyter.</span><span class="sxs-lookup"><span data-stu-id="ff05e-213">Then, import the results into a data frame to plot the target variables and prospective features for visual inspection by using the auto-visualization feature of Jupyter.</span></span>

### <a name="use-local-and-sql-magic-to-plot-data"></a><span data-ttu-id="ff05e-214">Použít místní a SQL magic k vykreslení dat</span><span class="sxs-lookup"><span data-stu-id="ff05e-214">Use local and SQL magic to plot data</span></span>
<span data-ttu-id="ff05e-215">Ve výchozím nastavení je k dispozici v kontextu relace, která jsou ukládána na pracovních uzlech výstup všech fragment kódu, který spustíte z poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="ff05e-215">By default, the output of any code snippet that you run from a Jupyter notebook is available within the context of the session that is persisted on the worker nodes.</span></span> <span data-ttu-id="ff05e-216">Pokud chcete uložit cestě k pracovním uzlům pro každý výpočty, a pokud všechna data, je třeba vaše výpočetní je dostupný místně na uzel serveru Jupyter (což je hlavního uzlu), můžete použít `%%local` magic ke spuštění fragmentu kódu na Jupyter Server.</span><span class="sxs-lookup"><span data-stu-id="ff05e-216">If you want to save a trip to the worker nodes for every computation, and if all the data that you need for your computation is available locally on the Jupyter server node (which is the head node), you can use the `%%local` magic to run the code snippet on the Jupyter server.</span></span>

* <span data-ttu-id="ff05e-217">**SQL magic** (`%%sql`).</span><span class="sxs-lookup"><span data-stu-id="ff05e-217">**SQL magic** (`%%sql`).</span></span> <span data-ttu-id="ff05e-218">Jádro HDInsight Spark podporuje dotazy na snadno vložené HiveQL pro SQLContext.</span><span class="sxs-lookup"><span data-stu-id="ff05e-218">The HDInsight Spark kernel supports easy inline HiveQL queries against SQLContext.</span></span> <span data-ttu-id="ff05e-219">(`-o VARIABLE_NAME`) Argument potrvají výstup příkazu jazyka SQL jako rámeček Pandas dat na serveru Jupyter.</span><span class="sxs-lookup"><span data-stu-id="ff05e-219">The (`-o VARIABLE_NAME`) argument persists the output of the SQL query as a Pandas data frame on the Jupyter server.</span></span> <span data-ttu-id="ff05e-220">To znamená, že budete mít k dispozici v místním režimu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-220">This means it'll be available in the local mode.</span></span>
* <span data-ttu-id="ff05e-221">`%%local`**magic**.</span><span class="sxs-lookup"><span data-stu-id="ff05e-221">`%%local` **magic**.</span></span> <span data-ttu-id="ff05e-222">`%%local` Magic běží kód místně na serveru Jupyter, což je hlavního uzlu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ff05e-222">The `%%local` magic runs the code locally on the Jupyter server, which is the head node of the HDInsight cluster.</span></span> <span data-ttu-id="ff05e-223">Obvykle použijete, `%%local` magic ve spojení s `%%sql` magic s `-o` parametr.</span><span class="sxs-lookup"><span data-stu-id="ff05e-223">Typically, you use `%%local` magic in conjunction with the `%%sql` magic with the `-o` parameter.</span></span> <span data-ttu-id="ff05e-224">`-o` Parametr by zachovat výstup příkazu jazyka SQL místně a potom `%%local` magic by aktivovat další sadu fragment kódu ke spouštění místně na výstupu dotazů SQL, který je místně trvalé.</span><span class="sxs-lookup"><span data-stu-id="ff05e-224">The `-o` parameter would persist the output of the SQL query locally, and then `%%local` magic would trigger the next set of code snippet to run locally against the output of the SQL queries that is persisted locally.</span></span>

### <a name="query-the-data-by-using-sql"></a><span data-ttu-id="ff05e-225">Dotaz na data pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="ff05e-225">Query the data by using SQL</span></span>
<span data-ttu-id="ff05e-226">Tento dotaz načte služebních cest taxíkem velikost tarif, osobní počet a velikost tip.</span><span class="sxs-lookup"><span data-stu-id="ff05e-226">This query retrieves the taxi trips by fare amount, passenger count, and tip amount.</span></span>

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

<span data-ttu-id="ff05e-227">V následujícím kódu `%%local` magic vytvoří místní data rámce, sqlResults.</span><span class="sxs-lookup"><span data-stu-id="ff05e-227">In the following code, the `%%local` magic creates a local data frame, sqlResults.</span></span> <span data-ttu-id="ff05e-228">SqlResults můžete použít k vykreslení pomocí matplotlib.</span><span class="sxs-lookup"><span data-stu-id="ff05e-228">You can use sqlResults to plot by using matplotlib.</span></span>

> [!TIP]
> <span data-ttu-id="ff05e-229">Místní magic se používá více než jednou. v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ff05e-229">Local magic is used multiple times in this article.</span></span> <span data-ttu-id="ff05e-230">Pokud je velké datové sady, prosím ukázkové vytvořit datové rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="ff05e-230">If your data set is large, please sample to create a data frame that can fit in local memory.</span></span>
> 
> 

### <a name="plot-the-data"></a><span data-ttu-id="ff05e-231">Vykreslení dat</span><span class="sxs-lookup"><span data-stu-id="ff05e-231">Plot the data</span></span>
<span data-ttu-id="ff05e-232">Můžete zobrazit pomocí kód Python po lokální kontext jako snímek dat Pandas rámečku data.</span><span class="sxs-lookup"><span data-stu-id="ff05e-232">You can plot by using Python code after the data frame is in local context as a Pandas data frame.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 <span data-ttu-id="ff05e-233">Spark jádra automaticky vizualizuje výstup dotazy SQL (HiveQL), po spuštění kódu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-233">The Spark kernel automatically visualizes the output of SQL (HiveQL) queries after you run the code.</span></span> <span data-ttu-id="ff05e-234">Můžete si vybrat mezi několik typů vizualizace:</span><span class="sxs-lookup"><span data-stu-id="ff05e-234">You can choose between several types of visualizations:</span></span>

* <span data-ttu-id="ff05e-235">Table</span><span class="sxs-lookup"><span data-stu-id="ff05e-235">Table</span></span>
* <span data-ttu-id="ff05e-236">Výsečový</span><span class="sxs-lookup"><span data-stu-id="ff05e-236">Pie</span></span>
* <span data-ttu-id="ff05e-237">Perokresba</span><span class="sxs-lookup"><span data-stu-id="ff05e-237">Line</span></span>
* <span data-ttu-id="ff05e-238">Oblast</span><span class="sxs-lookup"><span data-stu-id="ff05e-238">Area</span></span>
* <span data-ttu-id="ff05e-239">Panel</span><span class="sxs-lookup"><span data-stu-id="ff05e-239">Bar</span></span>

<span data-ttu-id="ff05e-240">Tady je kód k vykreslení dat:</span><span class="sxs-lookup"><span data-stu-id="ff05e-240">Here's the code to plot the data:</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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


<span data-ttu-id="ff05e-241">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-241">**Output:**</span></span>

![Tip velikost histogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tip velikost podle velikosti tarif](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a><span data-ttu-id="ff05e-245">Vytvoření funkce transformace funkce a potom připravená data data pro vstup do funkce modelování</span><span class="sxs-lookup"><span data-stu-id="ff05e-245">Create features and transform features, and then prep data for input into modeling functions</span></span>
<span data-ttu-id="ff05e-246">Pro funkce na základě stromu modelování ze Spark ML a MLlib budete muset připravit cílový a funkcí pomocí různých technik, jako například přihrádkování, indexování, jeden horkou kódování a vectorization.</span><span class="sxs-lookup"><span data-stu-id="ff05e-246">For tree-based modeling functions from Spark ML and MLlib, you have to prepare target and features by using a variety of techniques, such as binning, indexing, one-hot encoding, and vectorization.</span></span> <span data-ttu-id="ff05e-247">Zde je postup použijte v této části:</span><span class="sxs-lookup"><span data-stu-id="ff05e-247">Here are the procedures to follow in this section:</span></span>

1. <span data-ttu-id="ff05e-248">Vytvořit novou funkci ve **přihrádkování** čas do provozu časové intervaly.</span><span class="sxs-lookup"><span data-stu-id="ff05e-248">Create a new feature by **binning** hours into traffic time buckets.</span></span>
2. <span data-ttu-id="ff05e-249">Použít **horkou jeden kódování a indexování** kategorií funkcí.</span><span class="sxs-lookup"><span data-stu-id="ff05e-249">Apply **indexing and one-hot encoding** to categorical features.</span></span>
3. <span data-ttu-id="ff05e-250">**Ukázkové a rozdělení v datové sadě** do učení a testovací zlomků.</span><span class="sxs-lookup"><span data-stu-id="ff05e-250">**Sample and split the data set** into training and test fractions.</span></span>
4. <span data-ttu-id="ff05e-251">**Zadejte proměnnou školení a funkce**a poté vytvořit indexované nebo horkou jeden kódovaný školení a testování vstupní bod s popiskem odolné distribuovaných datové sady (RDDs) nebo datové rámce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-251">**Specify training variable and features**, and then create indexed or one-hot encoded training and testing input labeled point resilient distributed datasets (RDDs) or data frames.</span></span>
5. <span data-ttu-id="ff05e-252">Automaticky **kategorií a vectorize funkce a cíle** chcete použít jako vstupy pro modely machine learning.</span><span class="sxs-lookup"><span data-stu-id="ff05e-252">Automatically **categorize and vectorize features and targets** to use as inputs for machine learning models.</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="ff05e-253">Vytvořit novou funkci přihrádkování čas do provozu čas intervalů</span><span class="sxs-lookup"><span data-stu-id="ff05e-253">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="ff05e-254">Tento kód ukazuje, jak vytvořit novou funkci přihrádkování čas do kbelíků dobu provozu a jak pro ukládání do mezipaměti výsledné datové rámce v paměti.</span><span class="sxs-lookup"><span data-stu-id="ff05e-254">This code shows you how to create a new feature by binning hours into traffic time buckets and how to cache the resulting data frame in memory.</span></span> <span data-ttu-id="ff05e-255">Kde rámce RDDs a data se používají opakovaně, ukládání do mezipaměti vede k lepší časy spuštění.</span><span class="sxs-lookup"><span data-stu-id="ff05e-255">Where RDDs and data frames are used repeatedly, caching leads to improved execution times.</span></span> <span data-ttu-id="ff05e-256">V několika fázích v následujících postupech budete odpovídajícím způsobem, mezipaměti RDDs a datové rámce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-256">Accordingly, you'll cache RDDs and data frames at several stages in the following procedures.</span></span>

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

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a><span data-ttu-id="ff05e-257">Indexování a jeden horkou kódování kategorií funkcí</span><span class="sxs-lookup"><span data-stu-id="ff05e-257">Indexing and one-hot encoding of categorical features</span></span>
<span data-ttu-id="ff05e-258">Modelování a předvídání funkce MLlib vyžadují funkce s kategorií vstupní data na indexované nebo kódovaný před použití.</span><span class="sxs-lookup"><span data-stu-id="ff05e-258">The modeling and predict functions of MLlib require features with categorical input data to be indexed or encoded prior to use.</span></span> <span data-ttu-id="ff05e-259">V této části ukazuje, jak index nebo kódování kategorií funkce pro vstup do funkce modelování.</span><span class="sxs-lookup"><span data-stu-id="ff05e-259">This section shows you how to index or encode categorical features for input into the modeling functions.</span></span>

<span data-ttu-id="ff05e-260">Budete muset index nebo kódování modely různými způsoby v závislosti na modelu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-260">You need to index or encode your models in different ways, depending on the model.</span></span> <span data-ttu-id="ff05e-261">Například logistic a lineární regrese modely vyžadovat horkou jeden kódování.</span><span class="sxs-lookup"><span data-stu-id="ff05e-261">For example, logistic and linear regression models require one-hot encoding.</span></span> <span data-ttu-id="ff05e-262">Například můžete do tři sloupce funkce rozšířit funkce s tří kategorií.</span><span class="sxs-lookup"><span data-stu-id="ff05e-262">For example, a feature with three categories can be expanded into three feature columns.</span></span> <span data-ttu-id="ff05e-263">Každý sloupec obsahuje 0 nebo 1 v závislosti na kategorii pozorování.</span><span class="sxs-lookup"><span data-stu-id="ff05e-263">Each column would contain 0 or 1 depending on the category of an observation.</span></span> <span data-ttu-id="ff05e-264">Poskytuje MLlib [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce pro jeden horkou kódování.</span><span class="sxs-lookup"><span data-stu-id="ff05e-264">MLlib provides the [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function for one-hot encoding.</span></span> <span data-ttu-id="ff05e-265">Tato kodér mapuje sloupec popisek indexů ke sloupci binárního vektory s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-265">This encoder maps a column of label indices to a column of binary vectors with at most a single one-value.</span></span> <span data-ttu-id="ff05e-266">Toto kódování, lze použít algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression kategorií funkcí.</span><span class="sxs-lookup"><span data-stu-id="ff05e-266">With this encoding, algorithms that expect numerical valued features, such as logistic regression, can be applied to categorical features.</span></span>

<span data-ttu-id="ff05e-267">Zde transformace pouze čtyři proměnné, které chcete zobrazit příklady, které jsou řetězce znaků.</span><span class="sxs-lookup"><span data-stu-id="ff05e-267">Here you transform only four variables to show examples, which are character strings.</span></span> <span data-ttu-id="ff05e-268">Další proměnné, jako například den v týdnu, reprezentována číselné hodnoty, jako kategorií proměnné také mohou indexu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-268">You also can index other variables, such as weekday, represented by numerical values, as categorical variables.</span></span>

<span data-ttu-id="ff05e-269">Pro indexování, použijte `StringIndexer()`a pro jeden horkou kódování, použijte `OneHotEncoder()` funkce z MLlib.</span><span class="sxs-lookup"><span data-stu-id="ff05e-269">For indexing, use `StringIndexer()`, and for one-hot encoding, use `OneHotEncoder()` functions from MLlib.</span></span> <span data-ttu-id="ff05e-270">Tady je kód pro index a kódování kategorií funkce:</span><span class="sxs-lookup"><span data-stu-id="ff05e-270">Here is the code to index and encode categorical features:</span></span>

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
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

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="ff05e-271">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-271">**Output:**</span></span>

<span data-ttu-id="ff05e-272">Čas spuštění buňky: 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="ff05e-272">Time to run the cell: 4 seconds.</span></span>

### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a><span data-ttu-id="ff05e-273">Ukázka a rozdělení datové sady do učení a testovací zlomků</span><span class="sxs-lookup"><span data-stu-id="ff05e-273">Sample and split the data set into training and test fractions</span></span>
<span data-ttu-id="ff05e-274">Tento kód vytvoří náhodné vzorky dat (v tomto příkladu 25 %).</span><span class="sxs-lookup"><span data-stu-id="ff05e-274">This code creates a random sampling of the data (25%, in this example).</span></span> <span data-ttu-id="ff05e-275">I když vzorkování není potřeba v tomto příkladu kvůli překročení velikosti datové sady, článek ukazuje, jak můžete zkusit, abyste věděli, jak používat vlastní problémů v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="ff05e-275">Although sampling is not required for this example due to the size of the data set, the article shows you how you can sample so that you know how to use it for your own problems when needed.</span></span> <span data-ttu-id="ff05e-276">Po velká vzorky lze ušetřit čas významné během cvičení modelů.</span><span class="sxs-lookup"><span data-stu-id="ff05e-276">When samples are large, this can save significant time while you train models.</span></span> <span data-ttu-id="ff05e-277">Dále rozdělte vzorku na školení část (v tomto příkladu 75 %) a testování část (v tomto příkladu 25 %) pro použití v klasifikaci a regresní modelování.</span><span class="sxs-lookup"><span data-stu-id="ff05e-277">Next, split the sample into a training part (75%, in this example) and a testing part (25%, in this example) to use in classification and regression modeling.</span></span>

<span data-ttu-id="ff05e-278">Přidáte na každý řádek (ve sloupci "rand –"), který slouží k výběru křížové ověření složení během cvičení náhodné číslo (mezi 0 a 1).</span><span class="sxs-lookup"><span data-stu-id="ff05e-278">Add a random number (between 0 and 1) to each row (in a "rand" column) that can be used to select cross-validation folds during training.</span></span>

    # RECORD THE START TIME
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

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="ff05e-279">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-279">**Output:**</span></span>

<span data-ttu-id="ff05e-280">Čas spuštění buňky: 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="ff05e-280">Time to run the cell: 2 seconds.</span></span>

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a><span data-ttu-id="ff05e-281">Zadejte proměnnou školení a funkce a pak vytvořte indexované nebo jeden horkou kódovaný trénování a testování vstup s názvem bez přípony bodu RDDs nebo data rámce</span><span class="sxs-lookup"><span data-stu-id="ff05e-281">Specify training variable and features, and then create indexed or one-hot encoded training and testing input labeled point RDDs or data frames</span></span>
<span data-ttu-id="ff05e-282">Tato část obsahuje kód, který ukazuje, jak indexu kategorií textová data jako datový typ s popiskem bodu a jeho kódování, takže ho můžete a natrénuje a otestuje MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="ff05e-282">This section contains code that shows you how to index categorical text data as a labeled point data type, and encode it so you can use it to train and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="ff05e-283">S popiskem bodu objekty jsou RDDs naformátované způsobem, který je nutný jako vstupní data pro většinu v MLlib algoritmů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="ff05e-283">Labeled point objects are RDDs that are formatted in a way that is needed as input data by most of machine learning algorithms in MLlib.</span></span> <span data-ttu-id="ff05e-284">A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.</span><span class="sxs-lookup"><span data-stu-id="ff05e-284">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="ff05e-285">V tento kód zadejte Cílová proměnná (závislé) a funkce, které chcete použít k trénování modely.</span><span class="sxs-lookup"><span data-stu-id="ff05e-285">In this code, you specify the target (dependent) variable and the features to use to train models.</span></span> <span data-ttu-id="ff05e-286">Pak vytvoříte indexované nebo jeden horkou kódovaný trénování a testování vstup s názvem bez přípony bodu RDDs nebo data rámce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-286">Then, you create indexed or one-hot encoded training and testing input labeled point RDDs or data frames.</span></span>

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="ff05e-287">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-287">**Output:**</span></span>

<span data-ttu-id="ff05e-288">Čas spuštění buňky: 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="ff05e-288">Time to run the cell: 4 seconds.</span></span>

### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a><span data-ttu-id="ff05e-289">Automaticky kategorií a vectorize funkce a cíle, které chcete použít jako vstupy pro modelů strojového učení</span><span class="sxs-lookup"><span data-stu-id="ff05e-289">Automatically categorize and vectorize features and targets to use as inputs for machine learning models</span></span>
<span data-ttu-id="ff05e-290">Použijte Spark ML ke kategorizaci cíle a funkce, které chcete použít na základě stromu modelování funkcí.</span><span class="sxs-lookup"><span data-stu-id="ff05e-290">Use Spark ML to categorize the target and features to use in tree-based modeling functions.</span></span> <span data-ttu-id="ff05e-291">Kód dokončení dvě úlohy:</span><span class="sxs-lookup"><span data-stu-id="ff05e-291">The code completes two tasks:</span></span>

* <span data-ttu-id="ff05e-292">Vytvoří binární cíl pro klasifikaci přiřazením hodnoty 0 nebo 1 pro každý datový bod mezi 0 a 1 pomocí prahovou hodnotu 0,5.</span><span class="sxs-lookup"><span data-stu-id="ff05e-292">Creates a binary target for classification by assigning a value of 0 or 1 to each data point between 0 and 1 by using a threshold value of 0.5.</span></span>
* <span data-ttu-id="ff05e-293">Automaticky rozděluje funkce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-293">Automatically categorizes features.</span></span> <span data-ttu-id="ff05e-294">Pokud počet jedinečných hodnot číselné u všech funkcí je menší než 32, je zařazený do kategorie této funkce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-294">If the number of distinct numerical values for any feature is less than 32, that feature is categorized.</span></span>

<span data-ttu-id="ff05e-295">Zde je kód pro tyto dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="ff05e-295">Here's the code for these two tasks.</span></span>

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

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

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a><span data-ttu-id="ff05e-296">Model binární klasifikace: předpovědět, zda by měl být placené tip</span><span class="sxs-lookup"><span data-stu-id="ff05e-296">Binary classification model: Predict whether a tip should be paid</span></span>
<span data-ttu-id="ff05e-297">V této části můžete vytvořit tři typy binární klasifikace modely k předvídání, zda by měl být placené tip:</span><span class="sxs-lookup"><span data-stu-id="ff05e-297">In this section, you create three types of binary classification models to predict whether or not a tip should be paid:</span></span>

* <span data-ttu-id="ff05e-298">A **logistic regresní model** pomocí Spark ML `LogisticRegression()` – funkce</span><span class="sxs-lookup"><span data-stu-id="ff05e-298">A **logistic regression model** by using the Spark ML `LogisticRegression()` function</span></span>
* <span data-ttu-id="ff05e-299">A **model klasifikace náhodných doménové struktury** pomocí Spark ML `RandomForestClassifier()` – funkce</span><span class="sxs-lookup"><span data-stu-id="ff05e-299">A **random forest classification model** by using the Spark ML `RandomForestClassifier()` function</span></span>
* <span data-ttu-id="ff05e-300">A **přechodu zvýšení skóre modelu klasifikace stromu** pomocí MLlib `GradientBoostedTrees()` – funkce</span><span class="sxs-lookup"><span data-stu-id="ff05e-300">A **gradient boosting tree classification model** by using the MLlib `GradientBoostedTrees()` function</span></span>

### <a name="create-a-logistic-regression-model"></a><span data-ttu-id="ff05e-301">Vytvoření modelu logistic regression</span><span class="sxs-lookup"><span data-stu-id="ff05e-301">Create a logistic regression model</span></span>
<span data-ttu-id="ff05e-302">Dále vytvořte logistic regresní model pomocí Spark ML `LogisticRegression()` funkce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-302">Next, create a logistic regression model by using the Spark ML `LogisticRegression()` function.</span></span> <span data-ttu-id="ff05e-303">Vytvoříte model vytváření kódu v sérii kroků:</span><span class="sxs-lookup"><span data-stu-id="ff05e-303">You create the model building code in a series of steps:</span></span>

1. <span data-ttu-id="ff05e-304">**Trénování modelu** dat pomocí jednu sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="ff05e-304">**Train the model** data with one parameter set.</span></span>
2. <span data-ttu-id="ff05e-305">**Vyhodnocení modelu** na testovací datové sady s metriky.</span><span class="sxs-lookup"><span data-stu-id="ff05e-305">**Evaluate the model** on a test data set with metrics.</span></span>
3. <span data-ttu-id="ff05e-306">**Model uložte** v úložišti objektů Blob pro budoucí spotřeby.</span><span class="sxs-lookup"><span data-stu-id="ff05e-306">**Save the model** in Blob storage for future consumption.</span></span>
4. <span data-ttu-id="ff05e-307">**Modul score model** proti testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="ff05e-307">**Score the model** against test data.</span></span>
5. <span data-ttu-id="ff05e-308">**Vykreslení výsledky** s příjemce operační křivek vlastnosti (ROC).</span><span class="sxs-lookup"><span data-stu-id="ff05e-308">**Plot the results** with receiver operating characteristic (ROC) curves.</span></span>

<span data-ttu-id="ff05e-309">Tady je kód pro tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="ff05e-309">Here's the code for these procedures:</span></span>

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

<span data-ttu-id="ff05e-310">Zatížení, stanovení skóre a uložte výsledky.</span><span class="sxs-lookup"><span data-stu-id="ff05e-310">Load, score, and save the results.</span></span>

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


<span data-ttu-id="ff05e-311">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-311">**Output:**</span></span>

<span data-ttu-id="ff05e-312">ROC na testovací data = 0.9827381497557599</span><span class="sxs-lookup"><span data-stu-id="ff05e-312">ROC on test data = 0.9827381497557599</span></span>

<span data-ttu-id="ff05e-313">K vykreslení křivka ROC použijte jazyk Python na místní Pandas datové rámce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-313">Use Python on local Pandas data frames to plot the ROC curve.</span></span>

    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
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


<span data-ttu-id="ff05e-314">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-314">**Output:**</span></span>

![Tip nebo žádné křivka ROC tipu](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a><span data-ttu-id="ff05e-316">Vytvoření modelu klasifikace náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="ff05e-316">Create a random forest classification model</span></span>
<span data-ttu-id="ff05e-317">Dále vytvořte model klasifikace náhodných doménové struktury pomocí Spark ML `RandomForestClassifier()` fungovat a pak vyhodnotit modelu na testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="ff05e-317">Next, create a random forest classification model by using the Spark ML `RandomForestClassifier()` function, and then evaluate the model on test data.</span></span>

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


<span data-ttu-id="ff05e-318">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-318">**Output:**</span></span>

<span data-ttu-id="ff05e-319">ROC na testovací data = 0.9847103571552683</span><span class="sxs-lookup"><span data-stu-id="ff05e-319">ROC on test data = 0.9847103571552683</span></span>

### <a name="create-a-gbt-classification-model"></a><span data-ttu-id="ff05e-320">Vytvoření modelu GBT klasifikace</span><span class="sxs-lookup"><span data-stu-id="ff05e-320">Create a GBT classification model</span></span>
<span data-ttu-id="ff05e-321">Dále vytvořte klasifikaci model GBT pomocí MLlib na `GradientBoostedTrees()` fungovat a pak vyhodnotit modelu na testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="ff05e-321">Next, create a GBT classification model by using MLlib's `GradientBoostedTrees()` function, and then evaluate the model on test data.</span></span>

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


<span data-ttu-id="ff05e-322">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-322">**Output:**</span></span>

<span data-ttu-id="ff05e-323">Oblasti v rámci křivka ROC: 0.9846895479241554</span><span class="sxs-lookup"><span data-stu-id="ff05e-323">Area under ROC curve: 0.9846895479241554</span></span>

## <a name="regression-model-predict-tip-amount"></a><span data-ttu-id="ff05e-324">Regresní model: předpovědi velikost tipu</span><span class="sxs-lookup"><span data-stu-id="ff05e-324">Regression model: Predict tip amount</span></span>
<span data-ttu-id="ff05e-325">V této části vytvoříte dva typy regrese modely k předvídání velikost tip:</span><span class="sxs-lookup"><span data-stu-id="ff05e-325">In this section, you create two types of regression models to predict the tip amount:</span></span>

* <span data-ttu-id="ff05e-326">A **model lineární regrese Vyřešeno** pomocí Spark ML `LinearRegression()` funkce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-326">A **regularized linear regression model** by using the Spark ML `LinearRegression()` function.</span></span> <span data-ttu-id="ff05e-327">Můžete uložit model a vyhodnocení modelu na testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="ff05e-327">You'll save the model and evaluate the model on test data.</span></span>
* <span data-ttu-id="ff05e-328">A **zvyšovat skóre přechodu stromu regresní model** pomocí Spark ML `GBTRegressor()` funkce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-328">A **gradient-boosting tree regression model** by using the Spark ML `GBTRegressor()` function.</span></span>

### <a name="create-a-regularized-linear-regression-model"></a><span data-ttu-id="ff05e-329">Vytvořit model lineární regrese Vyřešeno</span><span class="sxs-lookup"><span data-stu-id="ff05e-329">Create a regularized linear regression model</span></span>
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="ff05e-330">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-330">**Output:**</span></span>

<span data-ttu-id="ff05e-331">Čas spuštění buňky: 13 sekund.</span><span class="sxs-lookup"><span data-stu-id="ff05e-331">Time to run the cell: 13 seconds.</span></span>

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


<span data-ttu-id="ff05e-332">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-332">**Output:**</span></span>

<span data-ttu-id="ff05e-333">R – sqr na testovací data = 0.5960320470835743</span><span class="sxs-lookup"><span data-stu-id="ff05e-333">R-sqr on test data = 0.5960320470835743</span></span>

<span data-ttu-id="ff05e-334">V dalším kroku dotaz výsledky testů jako snímek dat a použít AutoVizWidget a matplotlib k vizualizaci ho.</span><span class="sxs-lookup"><span data-stu-id="ff05e-334">Next, query the test results as a data frame and use AutoVizWidget and matplotlib to visualize it.</span></span>

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

<span data-ttu-id="ff05e-335">Kód vytvoří místní data snímku z výstupu dotazu a ukazuje zeměpisný data.</span><span class="sxs-lookup"><span data-stu-id="ff05e-335">The code creates a local data frame from the query output and plots the data.</span></span> <span data-ttu-id="ff05e-336">`%%local` Magic vytvoří místní data rámce, `sqlResults`, který můžete použít k vykreslení s matplotlib.</span><span class="sxs-lookup"><span data-stu-id="ff05e-336">The `%%local` magic creates a local data frame, `sqlResults`, which you can use to plot with matplotlib.</span></span>

> [!NOTE]
> <span data-ttu-id="ff05e-337">Tato magic Spark se používá více než jednou. v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ff05e-337">This Spark magic is used multiple times in this article.</span></span> <span data-ttu-id="ff05e-338">Pokud je velké množství dat, by měl ukázkové vytvořit datové rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="ff05e-338">If the amount of data is large, you should sample to create a data frame that can fit in local memory.</span></span>
> 
> 

<span data-ttu-id="ff05e-339">Vytvořte pozemků pomocí Python matplotlib.</span><span class="sxs-lookup"><span data-stu-id="ff05e-339">Create plots by using Python matplotlib.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

<span data-ttu-id="ff05e-340">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-340">**Output:**</span></span>

![Tip velikost: skutečnost a předpokládaných](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a><span data-ttu-id="ff05e-342">Vytvoření GBT regresní model</span><span class="sxs-lookup"><span data-stu-id="ff05e-342">Create a GBT regression model</span></span>
<span data-ttu-id="ff05e-343">Vytvoření GBT regresní model pomocí Spark ML `GBTRegressor()` fungovat a pak vyhodnotit modelu na testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="ff05e-343">Create a GBT regression model by using the Spark ML `GBTRegressor()` function, and then evaluate the model on test data.</span></span>

<span data-ttu-id="ff05e-344">[Boosted přechodu stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="ff05e-344">[Gradient-boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="ff05e-345">GBTs cvičení stromů rozhodnutí interaktivně, aby se minimalizoval funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-345">GBTs train decision trees iteratively to minimize a loss function.</span></span> <span data-ttu-id="ff05e-346">GBTs můžete použít pro regresní a klasifikace.</span><span class="sxs-lookup"><span data-stu-id="ff05e-346">You can use GBTs for regression and classification.</span></span> <span data-ttu-id="ff05e-347">Se může zpracovat kategorií funkce, nevyžadují funkce škálování a můžete zaznamenat nonlinearities a interakce funkce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-347">They can handle categorical features, do not require feature scaling, and can capture nonlinearities and feature interactions.</span></span> <span data-ttu-id="ff05e-348">Také můžete je v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="ff05e-348">You also can use them in a multiclass-classification setting.</span></span>

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


<span data-ttu-id="ff05e-349">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-349">**Output:**</span></span>

<span data-ttu-id="ff05e-350">Je test R-sqr: 0.7655383534596654</span><span class="sxs-lookup"><span data-stu-id="ff05e-350">Test R-sqr is: 0.7655383534596654</span></span>

## <a name="advanced-modeling-utilities-for-optimization"></a><span data-ttu-id="ff05e-351">Pokročilé modelování nástrojů pro optimalizaci</span><span class="sxs-lookup"><span data-stu-id="ff05e-351">Advanced modeling utilities for optimization</span></span>
<span data-ttu-id="ff05e-352">V této části použijte nástroje learning počítače, které vývojáři často používají pro optimalizaci modelu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-352">In this section, you use machine learning utilities that developers frequently use for model optimization.</span></span> <span data-ttu-id="ff05e-353">Konkrétně můžete pomocí parametru (vymetání) komínů a křížové ověření optimalizovat modely machine learning třemi různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="ff05e-353">Specifically, you can optimize machine learning models three different ways by using parameter sweeping and cross-validation:</span></span>

* <span data-ttu-id="ff05e-354">Rozdělení dat do vlaku a ověření sady, optimalizovat modelu s použitím technologie hyper parametr (vymetání) komínů na trénovací sady a vyhodnocení na sadu ověření (lineární regrese)</span><span class="sxs-lookup"><span data-stu-id="ff05e-354">Split the data into train and validation sets, optimize the model by using hyper-parameter sweeping on a training set, and evaluate on a validation set (linear regression)</span></span>
* <span data-ttu-id="ff05e-355">Optimalizovat modelu s použitím křížové ověření a technologie hyper parametr komínů pomocí funkce CrossValidator Spark ML (binární klasifikace)</span><span class="sxs-lookup"><span data-stu-id="ff05e-355">Optimize the model by using cross-validation and hyper-parameter sweeping by using Spark ML's CrossValidator function (binary classification)</span></span>
* <span data-ttu-id="ff05e-356">Optimalizovat modelu s použitím vlastní kód křížové ověření a parametr sweeping používat všechny strojového učení sadu funkce a parametr (lineární regrese)</span><span class="sxs-lookup"><span data-stu-id="ff05e-356">Optimize the model by using custom cross-validation and parameter-sweeping code to use any machine learning function and parameter set (linear regression)</span></span>

<span data-ttu-id="ff05e-357">**Křížové ověření** je technika, který vyhodnocuje, jak dobře model trénink na známé sadu dat bude generalize k předvídání funkce datové sady, na kterých je nebyla cvičena.</span><span class="sxs-lookup"><span data-stu-id="ff05e-357">**Cross-validation** is a technique that assesses how well a model trained on a known set of data will generalize to predict the features of data sets on which it has not been trained.</span></span> <span data-ttu-id="ff05e-358">Obecné cílem tato technika je cvičení modelu na datové sady známých dat, a pak je testován přesnost jeho předpovědi vůči nezávislé datové sady.</span><span class="sxs-lookup"><span data-stu-id="ff05e-358">The general idea behind this technique is that a model is trained on a data set of known data, and then the accuracy of its predictions is tested against an independent data set.</span></span> <span data-ttu-id="ff05e-359">Běžná implementace je rozdělit datové sady do *tisíc*-složení a potom trénování modelu v kruhového dotazování na všechny kromě jednoho složení.</span><span class="sxs-lookup"><span data-stu-id="ff05e-359">A common implementation is to divide a data set into *k*-folds, and then train the model in a round-robin fashion on all but one of the folds.</span></span>

<span data-ttu-id="ff05e-360">**Technologie Hyper parametr optimalizace** je tento problém vybrat sadu technologie hyper parametrů pro algoritmu učení, obvykle s cílem optimalizace měření výkonu algoritmus na nezávislé datové sady.</span><span class="sxs-lookup"><span data-stu-id="ff05e-360">**Hyper-parameter optimization** is the problem of choosing a set of hyper-parameters for a learning algorithm, usually with the goal of optimizing a measure of the algorithm's performance on an independent data set.</span></span> <span data-ttu-id="ff05e-361">Technologie hyper parametr je hodnota, která je nutné zadat mimo proces školení modelu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-361">A hyper-parameter is a value that you must specify outside the model training procedure.</span></span> <span data-ttu-id="ff05e-362">Předpoklady o technologie hyper parametr hodnoty může ovlivnit flexibilitu a přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-362">Assumptions about hyper-parameter values can affect the flexibility and accuracy of the model.</span></span> <span data-ttu-id="ff05e-363">Rozhodovací stromy například mít technologie hyper parametry, jako je například požadovaný hloubkou a počet nechá ve stromu.</span><span class="sxs-lookup"><span data-stu-id="ff05e-363">Decision trees have hyper-parameters, for example, such as the desired depth and number of leaves in the tree.</span></span> <span data-ttu-id="ff05e-364">Je nutné nastavit termín snížení chybnou klasifikaci pro podporu vektoru počítač (SVM).</span><span class="sxs-lookup"><span data-stu-id="ff05e-364">You must set a misclassification penalty term for a support vector machine (SVM).</span></span>

<span data-ttu-id="ff05e-365">Běžný způsob provedení optimalizace hyper parametr je používání funkce Hledat mřížky, označované taky jako **oblouku parametr**.</span><span class="sxs-lookup"><span data-stu-id="ff05e-365">A common way to perform hyper-parameter optimization is to use a grid search, also called a **parameter sweep**.</span></span> <span data-ttu-id="ff05e-366">V hledání mřížky provádí podrobné prohledávání prostřednictvím hodnoty podmnožinu zadaný hyper parametr prostoru pro algoritmus učení.</span><span class="sxs-lookup"><span data-stu-id="ff05e-366">In a grid search, an exhaustive search is performed through the values of a specified subset of the hyper-parameter space for a learning algorithm.</span></span> <span data-ttu-id="ff05e-367">Křížového ověření může poskytovat metriky výkonu seřadit optimální výsledky vyprodukované vyhledávací algoritmus mřížky.</span><span class="sxs-lookup"><span data-stu-id="ff05e-367">Cross-validation can supply a performance metric to sort out the optimal results produced by the grid search algorithm.</span></span> <span data-ttu-id="ff05e-368">Pokud používáte křížové ověření (technologie hyper parametr vymetání) komínů, můžete pomoct limit problémy jako overfitting modelu pro Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="ff05e-368">If you use cross-validation hyper-parameter sweeping, you can help limit problems like overfitting a model to training data.</span></span> <span data-ttu-id="ff05e-369">Tímto způsobem modelu zachová kapacity, které chcete použít pro obecné sadu dat, ze kterého jste extrahovali Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="ff05e-369">This way, the model retains the capacity to apply to the general set of data from which the training data was extracted.</span></span>

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a><span data-ttu-id="ff05e-370">Optimalizace model lineární regrese s hyper parametr (vymetání) komínů</span><span class="sxs-lookup"><span data-stu-id="ff05e-370">Optimize a linear regression model with hyper-parameter sweeping</span></span>
<span data-ttu-id="ff05e-371">Dále rozdělit data do vlaku a ověření sad, použití technologie hyper parametr komínů na trénovací sady k optimalizaci modelu a vyhodnocení na sadu ověření (lineární regrese).</span><span class="sxs-lookup"><span data-stu-id="ff05e-371">Next, split data into train and validation sets, use hyper-parameter sweeping on a training set to optimize the model, and evaluate on a validation set (linear regression).</span></span>

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


<span data-ttu-id="ff05e-372">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-372">**Output:**</span></span>

<span data-ttu-id="ff05e-373">Je test R-sqr: 0.6226484708501209</span><span class="sxs-lookup"><span data-stu-id="ff05e-373">Test R-sqr is: 0.6226484708501209</span></span>

### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a><span data-ttu-id="ff05e-374">Optimalizovat binární klasifikace modelu s použitím (vymetání) křížové ověření a technologie hyper parametr komínů</span><span class="sxs-lookup"><span data-stu-id="ff05e-374">Optimize the binary classification model by using cross-validation and hyper-parameter sweeping</span></span>
<span data-ttu-id="ff05e-375">V této části ukazuje, jak optimalizovat binární klasifikace modelu s použitím sweeping křížové ověření a technologie hyper parametr.</span><span class="sxs-lookup"><span data-stu-id="ff05e-375">This section shows you how to optimize a binary classification model by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="ff05e-376">Tato služba využívá Spark ML `CrossValidator` funkce.</span><span class="sxs-lookup"><span data-stu-id="ff05e-376">This uses the Spark ML `CrossValidator` function.</span></span>

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="ff05e-377">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-377">**Output:**</span></span>

<span data-ttu-id="ff05e-378">Čas spuštění buňky: 33 sekund.</span><span class="sxs-lookup"><span data-stu-id="ff05e-378">Time to run the cell: 33 seconds.</span></span>

### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a><span data-ttu-id="ff05e-379">Optimalizovat model lineární regrese s použitím vlastní křížové ověření a parametr sweeping kód</span><span class="sxs-lookup"><span data-stu-id="ff05e-379">Optimize the linear regression model by using custom cross-validation and parameter-sweeping code</span></span>
<span data-ttu-id="ff05e-380">V dalším kroku optimalizovat modelu s použitím vlastního kódu a identifikovat nejlepší parametry modelu pomocí kritéria nejvyšší přesnost.</span><span class="sxs-lookup"><span data-stu-id="ff05e-380">Next, optimize the model by using custom code, and identify the best model parameters by using the criterion of highest accuracy.</span></span> <span data-ttu-id="ff05e-381">Pak vytvořte konečné modelu, vyhodnocení modelu na testovací data a uložit model v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="ff05e-381">Then, create the final model, evaluate the model on test data, and save the model in Blob storage.</span></span> <span data-ttu-id="ff05e-382">Nakonec načíst model, stanovení skóre testovací data a vyhodnotit přesnost.</span><span class="sxs-lookup"><span data-stu-id="ff05e-382">Finally, load the model, score test data, and evaluate accuracy.</span></span>

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
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


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
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

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


<span data-ttu-id="ff05e-383">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="ff05e-383">**Output:**</span></span>

<span data-ttu-id="ff05e-384">Čas spuštění buňky: 61 sekund.</span><span class="sxs-lookup"><span data-stu-id="ff05e-384">Time to run the cell: 61 seconds.</span></span>

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a><span data-ttu-id="ff05e-385">Využívat modelů learning vytvořené Spark počítač automaticky pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="ff05e-385">Consume Spark-built machine learning models automatically with Scala</span></span>
<span data-ttu-id="ff05e-386">Přehled témata, která vás provede procesem úlohy, které tvoří proces vědecké zpracování dat v Azure najdete v tématu [proces vědecké účely dat Team](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="ff05e-386">For an overview of topics that walk you through the tasks that comprise the Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="ff05e-387">[Tým datové vědy proces návody](data-science-process-walkthroughs.md) popisuje další návody začátku do konce, které ukazují kroků v procesu vědecké účely Team dat u konkrétních scénářů.</span><span class="sxs-lookup"><span data-stu-id="ff05e-387">[Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) describes other end-to-end walkthroughs that demonstrate the steps in the Team Data Science Process for specific scenarios.</span></span> <span data-ttu-id="ff05e-388">Názorné postupy také ukazují, jak kombinovat cloudové a místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff05e-388">The walkthroughs also illustrate how to combine cloud and on-premises tools and services into a workflow or pipeline to create an intelligent application.</span></span>

<span data-ttu-id="ff05e-389">[Stanovení skóre modely vytvořené Spark strojové učení](machine-learning-data-science-spark-model-consumption.md) ukazuje, jak pomocí Scala kódu automaticky načíst a stanovíte jeho skóre nové sady dat s modely machine learning součástí Spark a uloží do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ff05e-389">[Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) shows you how to use Scala code to automatically load and score new data sets with machine learning models built in Spark and saved in Azure Blob storage.</span></span> <span data-ttu-id="ff05e-390">Můžete podle podle pokynů k dispozici a jednoduše místo kód Python Scala kód v tomto článku automatizované spotřeby.</span><span class="sxs-lookup"><span data-stu-id="ff05e-390">You can follow the instructions provided there, and simply replace the Python code with Scala code in this article for automated consumption.</span></span>

