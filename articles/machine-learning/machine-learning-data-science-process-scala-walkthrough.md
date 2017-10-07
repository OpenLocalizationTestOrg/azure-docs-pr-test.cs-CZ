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
# <a name="data-science-using-scala-and-spark-on-azure"></a><span data-ttu-id="e3e97-103">Vědecké zkoumání dat pomocí Scala a Spark v Azure</span><span class="sxs-lookup"><span data-stu-id="e3e97-103">Data Science using Scala and Spark on Azure</span></span>
<span data-ttu-id="e3e97-104">Tento článek ukazuje, jak balíčky toouse Scala pro úkoly pod dohledem strojového učení s hello škálovatelné MLlib Spark a Spark ML v clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="e3e97-104">This article shows you how toouse Scala for supervised machine learning tasks with hello Spark scalable MLlib and Spark ML packages on an Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="e3e97-105">Provede vás prostřednictvím hello úloh, které tvoří hello [proces vědecké zpracování dat](http://aka.ms/datascienceprocess): přijímání dat a zkoumání, vizualizace, funkce analýzy, modelování a spotřeba modelu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-105">It walks you through hello tasks that constitute hello [Data Science process](http://aka.ms/datascienceprocess): data ingestion and exploration, visualization, feature engineering, modeling, and model consumption.</span></span> <span data-ttu-id="e3e97-106">modely Hello v článku hello obsahovat logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromy (GBTs), kromě tootwo běžné pod dohledem úkoly strojového učení:</span><span class="sxs-lookup"><span data-stu-id="e3e97-106">hello models in hello article include logistic and linear regression, random forests, and gradient-boosted trees (GBTs), in addition tootwo common supervised machine learning tasks:</span></span>

* <span data-ttu-id="e3e97-107">Regrese problému: předpovědi hello tip velikost ($) pro cestu taxíkem</span><span class="sxs-lookup"><span data-stu-id="e3e97-107">Regression problem: Prediction of hello tip amount ($) for a taxi trip</span></span>
* <span data-ttu-id="e3e97-108">Binární klasifikace: předpovědi tip nebo cesty taxíkem žádné tip (1 nebo 0)</span><span class="sxs-lookup"><span data-stu-id="e3e97-108">Binary classification: Prediction of tip or no tip (1/0) for a taxi trip</span></span>

<span data-ttu-id="e3e97-109">Hello modelování proces vyžaduje trénování a hodnocení na testovací datové sady a relevantní přesnost metriky.</span><span class="sxs-lookup"><span data-stu-id="e3e97-109">hello modeling process requires training and evaluation on a test data set and relevant accuracy metrics.</span></span> <span data-ttu-id="e3e97-110">V tomto článku, můžete informace, jak toostore tyto modely v Azure Blob storage a jak tooscore a vyhodnotit jejich prediktivní výkonu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-110">In this article, you can learn how toostore these models in Azure Blob storage and how tooscore and evaluate their predictive performance.</span></span> <span data-ttu-id="e3e97-111">Tento článek se týká také pokročilejší témata z jak toooptimize modelů pomocí (vymetání) křížové ověření a technologie hyper parametr komínů hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-111">This article also covers hello more advanced topics of how toooptimize models by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="e3e97-112">použít data Hello je ukázka hello 2013 NYC taxíkem služební cestě a tarif datových sad dostupná na Githubu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-112">hello data used is a sample of hello 2013 NYC taxi trip and fare data set available on GitHub.</span></span>

<span data-ttu-id="e3e97-113">[Scala](http://www.scala-lang.org/), jazyk založené na virtuálním počítači Java hello, integruje koncepty jazyka objektově orientované a funkční.</span><span class="sxs-lookup"><span data-stu-id="e3e97-113">[Scala](http://www.scala-lang.org/), a language based on hello Java virtual machine, integrates object-oriented and functional language concepts.</span></span> <span data-ttu-id="e3e97-114">Je škálovatelná jazyk, který je dobře hodí toodistributed zpracování v cloudu hello a běží na clustery Spark v Azure.</span><span class="sxs-lookup"><span data-stu-id="e3e97-114">It's a scalable language that is well suited toodistributed processing in hello cloud, and runs on Azure Spark clusters.</span></span>

<span data-ttu-id="e3e97-115">[Spark](http://spark.apache.org/) framework paralelní zpracování open source, který podporuje v paměti zpracovává tooboost hello výkon aplikací analýzy velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="e3e97-115">[Spark](http://spark.apache.org/) is an open-source parallel-processing framework that supports in-memory processing tooboost hello performance of big data analytics applications.</span></span> <span data-ttu-id="e3e97-116">modul zpracování Spark Hello je vytvořené pro rychlost, snadné použití a sofistikované analytics.</span><span class="sxs-lookup"><span data-stu-id="e3e97-116">hello Spark processing engine is built for speed, ease of use, and sophisticated analytics.</span></span> <span data-ttu-id="e3e97-117">Možnosti v paměti distribuované výpočtů Spark ho nastavit správnou volbu pro iterativní algoritmy v machine learning a grafů výpočty.</span><span class="sxs-lookup"><span data-stu-id="e3e97-117">Spark's in-memory distributed computation capabilities make it a good choice for iterative algorithms in machine learning and graph computations.</span></span> <span data-ttu-id="e3e97-118">Hello [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) balíček poskytuje jednotnou sadu vysoké úrovně rozhraní API vytvořená na základě dat snímků, které vám pomůžou vytvářet a ladit praktické strojového učení kanály.</span><span class="sxs-lookup"><span data-stu-id="e3e97-118">hello [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) package provides a uniform set of high-level APIs built on top of data frames that can help you create and tune practical machine learning pipelines.</span></span> <span data-ttu-id="e3e97-119">[MLlib](http://spark.apache.org/mllib/) je Spark škálovatelné machine learning knihovny, která přináší modelování možnosti toothis distribuovaném prostředí.</span><span class="sxs-lookup"><span data-stu-id="e3e97-119">[MLlib](http://spark.apache.org/mllib/) is Spark's scalable machine learning library, which brings modeling capabilities toothis distributed environment.</span></span>

<span data-ttu-id="e3e97-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) je nabídka Azure hostovaná hello Spark open source.</span><span class="sxs-lookup"><span data-stu-id="e3e97-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) is hello Azure-hosted offering of open-source Spark.</span></span> <span data-ttu-id="e3e97-121">Zahrnuje taky podporu poznámkové bloky Jupyter Scala na clusteru Spark hello a můžete spustit tootransform interaktivních dotazů Spark SQL, filtrovat a vizualizovat data uložená v úložišti objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="e3e97-121">It also includes support for Jupyter Scala notebooks on hello Spark cluster, and can run Spark SQL interactive queries tootransform, filter, and visualize data stored in Azure Blob storage.</span></span> <span data-ttu-id="e3e97-122">Hello Scala fragmenty kódu v tomto článku s hello řešení, které zobrazit relevantní pozemků hello toovisualize hello data spustit v nainstalovaná na clustery Spark hello poznámkové bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="e3e97-122">hello Scala code snippets in this article that provide hello solutions and show hello relevant plots toovisualize hello data run in Jupyter notebooks installed on hello Spark clusters.</span></span> <span data-ttu-id="e3e97-123">Hello modelování kroky v těchto tématech mít kódu této ukazuje, jak tootrain, vyhodnotit, uložit a používat každý typ modelu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-123">hello modeling steps in these topics have code that shows you how tootrain, evaluate, save, and consume each type of model.</span></span>

<span data-ttu-id="e3e97-124">Postup instalace Hello a kódu v tomto článku jsou pro Azure HDInsight 3.4 Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="e3e97-124">hello setup steps and code in this article are for Azure HDInsight 3.4 Spark 1.6.</span></span> <span data-ttu-id="e3e97-125">Ale hello kódu v tomto článku a v hello [poznámkového bloku Jupyter Scala](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) obecné a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="e3e97-125">However, hello code in this article and in hello [Scala Jupyter Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) are generic and should work on any Spark cluster.</span></span> <span data-ttu-id="e3e97-126">nastavení clusteru s podporou Hello a kroky správy může být mírně lišit od co se zobrazí v tomto článku, pokud nepoužíváte HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="e3e97-126">hello cluster setup and management steps might be slightly different from what is shown in this article if you are not using HDInsight Spark.</span></span>

> [!NOTE]
> <span data-ttu-id="e3e97-127">Téma, které ukazuje, jak toouse Python, nikoli Scala toocomplete úloh na začátku do konce proces vědecké zpracování dat, najdete v části [vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e3e97-127">For a topic that shows you how toouse Python rather than Scala toocomplete tasks for an end-to-end Data Science process, see [Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="e3e97-128">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e3e97-128">Prerequisites</span></span>
* <span data-ttu-id="e3e97-129">Musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e3e97-129">You must have an Azure subscription.</span></span> <span data-ttu-id="e3e97-130">Pokud jste již nemají, [získat bezplatnou zkušební verzi Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e3e97-130">If you do not already have one, [get an Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="e3e97-131">Je nutné hello toocomplete clusteru Azure HDInsight 3.4 Spark 1.6 následující postupy.</span><span class="sxs-lookup"><span data-stu-id="e3e97-131">You need an Azure HDInsight 3.4 Spark 1.6 cluster toocomplete hello following procedures.</span></span> <span data-ttu-id="e3e97-132">toocreate clusteru, najdete v pokynech hello v [Začínáme: Vytvořte Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="e3e97-132">toocreate a cluster, see hello instructions in [Get started: Create Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="e3e97-133">Nastavit hello clusteru typ a verze na hello **vybrat typ clusteru** nabídky.</span><span class="sxs-lookup"><span data-stu-id="e3e97-133">Set hello cluster type and version on hello **Select Cluster Type** menu.</span></span>

![Konfigurace typu clusteru HDInsight](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

<span data-ttu-id="e3e97-135">Popis hello NYC taxíkem cestě dat a pokyny, jak tooexecute kód z poznámkového bloku Jupyter v clusteru Spark hello najdete v tématu hello příslušné části v [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e3e97-135">For a description of hello NYC taxi trip data and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster, see hello relevant sections in [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a><span data-ttu-id="e3e97-136">Spustí kód Scala z poznámkového bloku Jupyter v clusteru Spark hello</span><span class="sxs-lookup"><span data-stu-id="e3e97-136">Execute Scala code from a Jupyter notebook on hello Spark cluster</span></span>
<span data-ttu-id="e3e97-137">Můžete spustit poznámkového bloku Jupyter z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e3e97-137">You can launch a Jupyter notebook from hello Azure portal.</span></span> <span data-ttu-id="e3e97-138">Najděte hello cluster Spark na řídicím panelu a klikněte na něj tooenter hello správu stránky pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="e3e97-138">Find hello Spark cluster on your dashboard, and then click it tooenter hello management page for your cluster.</span></span> <span data-ttu-id="e3e97-139">Klikněte na tlačítko **řídicí panely clusteru**a potom klikněte na **Poznámkový blok Jupyter** tooopen hello poznámkového bloku spojené s clusterem Spark hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-139">Next, click **Cluster Dashboards**, and then click **Jupyter Notebook** tooopen hello notebook associated with hello Spark cluster.</span></span>

![Řídicí panel clusteru a poznámkové bloky Jupyter](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

<span data-ttu-id="e3e97-141">Také můžete přístup poznámkové bloky Jupyter na https://&lt;clustername&gt;.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="e3e97-141">You also can access Jupyter notebooks at https://&lt;clustername&gt;.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="e3e97-142">Nahraďte *clustername* s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="e3e97-142">Replace *clustername* with hello name of your cluster.</span></span> <span data-ttu-id="e3e97-143">Potřebujete hello heslo pro váš správce účtu tooaccess hello poznámkové bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="e3e97-143">You need hello password for your administrator account tooaccess hello Jupyter notebooks.</span></span>

![Přejděte poznámkových bloků tooJupyter pomocí názvu clusteru hello](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

<span data-ttu-id="e3e97-145">Vyberte **Scala** toosee adresář, který má několik příkladů hotových poznámkových bloků, že použití hello PySpark rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3e97-145">Select **Scala** toosee a directory that has a few examples of prepackaged notebooks that use hello PySpark API.</span></span> <span data-ttu-id="e3e97-146">Hello modelování zkoumání a hodnocení pomocí poznámkového bloku Scala.ipynb, která obsahuje ukázky kódu hello této sady témat Spark je k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span><span class="sxs-lookup"><span data-stu-id="e3e97-146">hello Exploration Modeling and Scoring using Scala.ipynb notebook that contains hello code samples for this suite of Spark topics is available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span></span>

<span data-ttu-id="e3e97-147">Můžete nahrát poznámkového bloku hello přímo z Githubu toohello Poznámkový blok Jupyter serveru v clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="e3e97-147">You can upload hello notebook directly from GitHub toohello Jupyter Notebook server on your Spark cluster.</span></span> <span data-ttu-id="e3e97-148">Na domovské stránce Jupyter, klikněte na tlačítko hello **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e3e97-148">On your Jupyter home page, click hello **Upload** button.</span></span> <span data-ttu-id="e3e97-149">V Průzkumníku souborů hello, vložte adresu URL webu GitHub (nezpracovaná obsah) hello hello Scala poznámkového bloku a pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="e3e97-149">In hello file explorer, paste hello GitHub (raw content) URL of hello Scala notebook, and then click **Open**.</span></span> <span data-ttu-id="e3e97-150">Poznámkový blok Scala Hello je k dispozici na hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="e3e97-150">hello Scala notebook is available at hello following URL:</span></span>

[<span data-ttu-id="e3e97-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span><span class="sxs-lookup"><span data-stu-id="e3e97-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span></span>](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a><span data-ttu-id="e3e97-152">Instalace: Kontexty přednastavených Spark a Hive, Spark Magic a knihoven Spark</span><span class="sxs-lookup"><span data-stu-id="e3e97-152">Setup: Preset Spark and Hive contexts, Spark magics, and Spark libraries</span></span>
### <a name="preset-spark-and-hive-contexts"></a><span data-ttu-id="e3e97-153">Předvolby kontexty Spark a Hive</span><span class="sxs-lookup"><span data-stu-id="e3e97-153">Preset Spark and Hive contexts</span></span>
    # SET hello START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


<span data-ttu-id="e3e97-154">Hello Spark jádra, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontexty.</span><span class="sxs-lookup"><span data-stu-id="e3e97-154">hello Spark kernels that are provided with Jupyter notebooks have preset contexts.</span></span> <span data-ttu-id="e3e97-155">Kontexty Spark nebo Hive sady hello tooexplicitly nepotřebujete před zahájením práce s hello aplikací, které vyvíjíte.</span><span class="sxs-lookup"><span data-stu-id="e3e97-155">You don't need tooexplicitly set hello Spark or Hive contexts before you start working with hello application you are developing.</span></span> <span data-ttu-id="e3e97-156">Hello přednastavené kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="e3e97-156">hello preset contexts are:</span></span>

* <span data-ttu-id="e3e97-157">`sc`pro SparkContext</span><span class="sxs-lookup"><span data-stu-id="e3e97-157">`sc` for SparkContext</span></span>
* <span data-ttu-id="e3e97-158">`sqlContext`pro HiveContext</span><span class="sxs-lookup"><span data-stu-id="e3e97-158">`sqlContext` for HiveContext</span></span>

### <a name="spark-magics"></a><span data-ttu-id="e3e97-159">Spark Magic</span><span class="sxs-lookup"><span data-stu-id="e3e97-159">Spark magics</span></span>
<span data-ttu-id="e3e97-160">Hello Spark jádra poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s `%%`.</span><span class="sxs-lookup"><span data-stu-id="e3e97-160">hello Spark kernel provides some predefined “magics,” which are special commands that you can call with `%%`.</span></span> <span data-ttu-id="e3e97-161">Dva z těchto příkazů se používají v hello následující ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-161">Two of these commands are used in hello following code samples.</span></span>

* <span data-ttu-id="e3e97-162">`%%local`Určuje, že kód hello v dalších řádcích bude proveden místně.</span><span class="sxs-lookup"><span data-stu-id="e3e97-162">`%%local` specifies that hello code in subsequent lines will be executed locally.</span></span> <span data-ttu-id="e3e97-163">Hello kód musí být platný kód Scala.</span><span class="sxs-lookup"><span data-stu-id="e3e97-163">hello code must be valid Scala code.</span></span>
* <span data-ttu-id="e3e97-164">`%%sql -o <variable name>`provede dotaz Hive proti `sqlContext`.</span><span class="sxs-lookup"><span data-stu-id="e3e97-164">`%%sql -o <variable name>` executes a Hive query against `sqlContext`.</span></span> <span data-ttu-id="e3e97-165">Pokud hello `-o` parametr se předává, hello výsledek dotazu hello je uchován v hello `%%local` Scala kontextu jako snímek dat Spark.</span><span class="sxs-lookup"><span data-stu-id="e3e97-165">If hello `-o` parameter is passed, hello result of hello query is persisted in hello `%%local` Scala context as a Spark data frame.</span></span>

<span data-ttu-id="e3e97-166">Pro další informace o hello jádra pro poznámkové bloky Jupyter a jejich předdefinované "magics", volání s `%%` (například `%%local`), najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="e3e97-166">For more information about hello kernels for Jupyter notebooks and their predefined "magics" that you call with `%%` (for example, `%%local`), see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

### <a name="import-libraries"></a><span data-ttu-id="e3e97-167">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="e3e97-167">Import libraries</span></span>
<span data-ttu-id="e3e97-168">Importovat hello Spark, MLlib a další knihovny, které budete potřebovat pomocí hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="e3e97-168">Import hello Spark, MLlib, and other libraries you'll need by using hello following code.</span></span>

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


## <a name="data-ingestion"></a><span data-ttu-id="e3e97-169">Přijímání dat</span><span class="sxs-lookup"><span data-stu-id="e3e97-169">Data ingestion</span></span>
<span data-ttu-id="e3e97-170">Hello prvním krokem v procesu vědecké zpracování dat hello je tooingest hello dat, které chcete tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="e3e97-170">hello first step in hello Data Science process is tooingest hello data that you want tooanalyze.</span></span> <span data-ttu-id="e3e97-171">Přepnutím hello data z externích zdrojů nebo systémy němž je umístěna do vašeho prostředí zkoumání a modelování data.</span><span class="sxs-lookup"><span data-stu-id="e3e97-171">You bring hello data from external sources or systems where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="e3e97-172">V tomto článku hello data, která jste ingestování je připojený k ukázka 0,1 % souboru hello taxíkem služební cestě a tarif (uložený jako soubor TSV).</span><span class="sxs-lookup"><span data-stu-id="e3e97-172">In this article, hello data you ingest is a joined 0.1% sample of hello taxi trip and fare file (stored as a .tsv file).</span></span> <span data-ttu-id="e3e97-173">prostředí pro zkoumání a modelování datového Hello je Spark.</span><span class="sxs-lookup"><span data-stu-id="e3e97-173">hello data exploration and modeling environment is Spark.</span></span> <span data-ttu-id="e3e97-174">Tato část obsahuje hello kód toocomplete hello následující řadu úloh:</span><span class="sxs-lookup"><span data-stu-id="e3e97-174">This section contains hello code toocomplete hello following series of tasks:</span></span>

1. <span data-ttu-id="e3e97-175">Nastavit cesty adresáře pro data a modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e3e97-175">Set directory paths for data and model storage.</span></span>
2. <span data-ttu-id="e3e97-176">Přečtěte si v hello vstupní datové sady (uložený jako soubor TSV).</span><span class="sxs-lookup"><span data-stu-id="e3e97-176">Read in hello input data set (stored as a .tsv file).</span></span>
3. <span data-ttu-id="e3e97-177">Definujte schéma pro hello data a vyčištění hello data.</span><span class="sxs-lookup"><span data-stu-id="e3e97-177">Define a schema for hello data and clean hello data.</span></span>
4. <span data-ttu-id="e3e97-178">Vytvořte rámeček vyčištěnými dat a uložení do mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="e3e97-178">Create a cleaned data frame and cache it in memory.</span></span>
5. <span data-ttu-id="e3e97-179">Zaregistrujte hello data jako do dočasné tabulky v SQLContext.</span><span class="sxs-lookup"><span data-stu-id="e3e97-179">Register hello data as a temporary table in SQLContext.</span></span>
6. <span data-ttu-id="e3e97-180">Dotaz na tabulku hello a importovat hello výsledky do rámečku data.</span><span class="sxs-lookup"><span data-stu-id="e3e97-180">Query hello table and import hello results into a data frame.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a><span data-ttu-id="e3e97-181">Nastavení cesty adresáře pro umístění úložiště v Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="e3e97-181">Set directory paths for storage locations in Azure Blob storage</span></span>
<span data-ttu-id="e3e97-182">Spark můžete číst a zapisovat tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e3e97-182">Spark can read and write tooAzure Blob storage.</span></span> <span data-ttu-id="e3e97-183">Můžete použít Spark tooprocess stávající data a pak uložte výsledky hello znovu v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e3e97-183">You can use Spark tooprocess any of your existing data, and then store hello results again in Blob storage.</span></span>

<span data-ttu-id="e3e97-184">modely toosave nebo souborů v Blob storage, musíte tooproperly zadejte cestu hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-184">toosave models or files in Blob storage, you need tooproperly specify hello path.</span></span> <span data-ttu-id="e3e97-185">Referenční dokumentace hello výchozí kontejner připojit toohello Spark cluster pomocí cesty, který začíná `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="e3e97-185">Reference hello default container attached toohello Spark cluster by using a path that begins with `wasb:///`.</span></span> <span data-ttu-id="e3e97-186">Referenční jiných umístění pomocí `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="e3e97-186">Reference other locations by using `wasb://`.</span></span>

<span data-ttu-id="e3e97-187">Hello následující ukázka kódu určuje umístění hello toobe hello vstupní data pro čtení a hello cesta tooBlob úložiště, které je připojené toohello clusteru Spark pro uložení hello modelu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-187">hello following code sample specifies hello location of hello input data toobe read and hello path tooBlob storage that is attached toohello Spark cluster where hello model will be saved.</span></span>

    # SET PATHS tooDATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET hello MODEL STORAGE DIRECTORY PATH
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-toohello-schema"></a><span data-ttu-id="e3e97-188">Umožňuje importovat data, RDD vytvořit a definovat rámeček dat podle schématu toohello</span><span class="sxs-lookup"><span data-stu-id="e3e97-188">Import data, create an RDD, and define a data frame according toohello schema</span></span>
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


<span data-ttu-id="e3e97-189">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-189">**Output:**</span></span>

<span data-ttu-id="e3e97-190">Čas toorun hello buňky: 8 sekund.</span><span class="sxs-lookup"><span data-stu-id="e3e97-190">Time toorun hello cell: 8 seconds.</span></span>

### <a name="query-hello-table-and-import-results-in-a-data-frame"></a><span data-ttu-id="e3e97-191">Dotaz na tabulku hello a importovat výsledky v rámci dat</span><span class="sxs-lookup"><span data-stu-id="e3e97-191">Query hello table and import results in a data frame</span></span>
<span data-ttu-id="e3e97-192">Další tabulka hello dotazu pro tarif, osobní a tip data; filtrování dat poškozen a odlehlé; a tisk několik řádků.</span><span class="sxs-lookup"><span data-stu-id="e3e97-192">Next, query hello table for fare, passenger, and tip data; filter out corrupt and outlying data; and print several rows.</span></span>

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

<span data-ttu-id="e3e97-193">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-193">**Output:**</span></span>

| <span data-ttu-id="e3e97-194">fare_amount</span><span class="sxs-lookup"><span data-stu-id="e3e97-194">fare_amount</span></span> | <span data-ttu-id="e3e97-195">passenger_count</span><span class="sxs-lookup"><span data-stu-id="e3e97-195">passenger_count</span></span> | <span data-ttu-id="e3e97-196">tip_amount</span><span class="sxs-lookup"><span data-stu-id="e3e97-196">tip_amount</span></span> | <span data-ttu-id="e3e97-197">vysypávány</span><span class="sxs-lookup"><span data-stu-id="e3e97-197">tipped</span></span> |
| --- | --- | --- | --- |
|        <span data-ttu-id="e3e97-198">13.5</span><span class="sxs-lookup"><span data-stu-id="e3e97-198">13.5</span></span> |<span data-ttu-id="e3e97-199">1.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-199">1.0</span></span> |<span data-ttu-id="e3e97-200">2.9</span><span class="sxs-lookup"><span data-stu-id="e3e97-200">2.9</span></span> |<span data-ttu-id="e3e97-201">1.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-201">1.0</span></span> |
|        <span data-ttu-id="e3e97-202">16.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-202">16.0</span></span> |<span data-ttu-id="e3e97-203">2.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-203">2.0</span></span> |<span data-ttu-id="e3e97-204">3.4</span><span class="sxs-lookup"><span data-stu-id="e3e97-204">3.4</span></span> |<span data-ttu-id="e3e97-205">1.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-205">1.0</span></span> |
|        <span data-ttu-id="e3e97-206">10.5</span><span class="sxs-lookup"><span data-stu-id="e3e97-206">10.5</span></span> |<span data-ttu-id="e3e97-207">2.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-207">2.0</span></span> |<span data-ttu-id="e3e97-208">1.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-208">1.0</span></span> |<span data-ttu-id="e3e97-209">1.0</span><span class="sxs-lookup"><span data-stu-id="e3e97-209">1.0</span></span> |

## <a name="data-exploration-and-visualization"></a><span data-ttu-id="e3e97-210">Zkoumání dat a vizualizaci</span><span class="sxs-lookup"><span data-stu-id="e3e97-210">Data exploration and visualization</span></span>
<span data-ttu-id="e3e97-211">Po přepnutí hello data do Spark, hello dalším krokem v hello proces vědecké zpracování dat je toogain podrobnější vysvětlení hello dat prostřednictvím zkoumání a vizualizace.</span><span class="sxs-lookup"><span data-stu-id="e3e97-211">After you bring hello data into Spark, hello next step in hello Data Science process is toogain a deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="e3e97-212">V této části Zkontrolujte hello taxíkem dat pomocí dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="e3e97-212">In this section, you examine hello taxi data by using SQL queries.</span></span> <span data-ttu-id="e3e97-213">Potom hello výsledky importu do tooplot rámečkem data hello cílových proměnných a potenciální funkcí pro visual kontroly pomocí funkce Automatické vizualizace hello Jupyter.</span><span class="sxs-lookup"><span data-stu-id="e3e97-213">Then, import hello results into a data frame tooplot hello target variables and prospective features for visual inspection by using hello auto-visualization feature of Jupyter.</span></span>

### <a name="use-local-and-sql-magic-tooplot-data"></a><span data-ttu-id="e3e97-214">Použít místní a magic tooplot dat SQL</span><span class="sxs-lookup"><span data-stu-id="e3e97-214">Use local and SQL magic tooplot data</span></span>
<span data-ttu-id="e3e97-215">Ve výchozím nastavení je k dispozici v rámci kontextu hello hello relace, který je na hello pracovní uzly trvalý výstup hello žádné fragment kódu, který lze spustit ze poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="e3e97-215">By default, hello output of any code snippet that you run from a Jupyter notebook is available within hello context of hello session that is persisted on hello worker nodes.</span></span> <span data-ttu-id="e3e97-216">Pokud chcete, aby toosave služební cestě toohello pracovním uzlům pro každý výpočty a pokud všechny hello data, která je nutné pro vaše výpočetní je k dispozici místně na uzel serveru Jupyter hello (což je hello hlavního uzlu), můžete použít hello `%%local` kouzelná toorun hello kódu fragment kódu na serveru Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-216">If you want toosave a trip toohello worker nodes for every computation, and if all hello data that you need for your computation is available locally on hello Jupyter server node (which is hello head node), you can use hello `%%local` magic toorun hello code snippet on hello Jupyter server.</span></span>

* <span data-ttu-id="e3e97-217">**SQL magic** (`%%sql`).</span><span class="sxs-lookup"><span data-stu-id="e3e97-217">**SQL magic** (`%%sql`).</span></span> <span data-ttu-id="e3e97-218">Hello HDInsight Spark jádra podporuje dotazy na snadno vložené HiveQL pro SQLContext.</span><span class="sxs-lookup"><span data-stu-id="e3e97-218">hello HDInsight Spark kernel supports easy inline HiveQL queries against SQLContext.</span></span> <span data-ttu-id="e3e97-219">Hello (`-o VARIABLE_NAME`) argument potrvají výstup hello dotazu SQL hello jako rámeček Pandas dat na serveru Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-219">hello (`-o VARIABLE_NAME`) argument persists hello output of hello SQL query as a Pandas data frame on hello Jupyter server.</span></span> <span data-ttu-id="e3e97-220">To znamená, že budete mít k dispozici v místním režimu hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-220">This means it'll be available in hello local mode.</span></span>
* <span data-ttu-id="e3e97-221">`%%local`**magic**.</span><span class="sxs-lookup"><span data-stu-id="e3e97-221">`%%local` **magic**.</span></span> <span data-ttu-id="e3e97-222">Hello `%%local` magic běží kód hello místně na serveru Jupyter hello, což je hello hlavního uzlu v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-222">hello `%%local` magic runs hello code locally on hello Jupyter server, which is hello head node of hello HDInsight cluster.</span></span> <span data-ttu-id="e3e97-223">Obvykle použijete, `%%local` magic ve spojení s hello `%%sql` magic s hello `-o` parametr.</span><span class="sxs-lookup"><span data-stu-id="e3e97-223">Typically, you use `%%local` magic in conjunction with hello `%%sql` magic with hello `-o` parameter.</span></span> <span data-ttu-id="e3e97-224">Hello `-o` parametr by zachovat hello výstup hello dotazu SQL místně a potom `%%local` magic by aktivovat hello další sadu toorun fragmentu kódu místně proti výstup hello hello dotazů SQL, který je místně trvalé.</span><span class="sxs-lookup"><span data-stu-id="e3e97-224">hello `-o` parameter would persist hello output of hello SQL query locally, and then `%%local` magic would trigger hello next set of code snippet toorun locally against hello output of hello SQL queries that is persisted locally.</span></span>

### <a name="query-hello-data-by-using-sql"></a><span data-ttu-id="e3e97-225">Dotaz na data hello pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="e3e97-225">Query hello data by using SQL</span></span>
<span data-ttu-id="e3e97-226">Tento dotaz načte služebních cest taxíkem hello velikost tarif, osobní počet a velikost tip.</span><span class="sxs-lookup"><span data-stu-id="e3e97-226">This query retrieves hello taxi trips by fare amount, passenger count, and tip amount.</span></span>

    # RUN hello SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

<span data-ttu-id="e3e97-227">V následujícím kódu hello, hello `%%local` magic vytvoří místní data rámce, sqlResults.</span><span class="sxs-lookup"><span data-stu-id="e3e97-227">In hello following code, hello `%%local` magic creates a local data frame, sqlResults.</span></span> <span data-ttu-id="e3e97-228">Můžete sqlResults tooplot pomocí matplotlib.</span><span class="sxs-lookup"><span data-stu-id="e3e97-228">You can use sqlResults tooplot by using matplotlib.</span></span>

> [!TIP]
> <span data-ttu-id="e3e97-229">Místní magic se používá více než jednou. v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e3e97-229">Local magic is used multiple times in this article.</span></span> <span data-ttu-id="e3e97-230">Pokud je velké datové sady, prosím ukázkové toocreate dat rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="e3e97-230">If your data set is large, please sample toocreate a data frame that can fit in local memory.</span></span>
> 
> 

### <a name="plot-hello-data"></a><span data-ttu-id="e3e97-231">Vykreslení dat hello</span><span class="sxs-lookup"><span data-stu-id="e3e97-231">Plot hello data</span></span>
<span data-ttu-id="e3e97-232">Můžete zobrazit pomocí kód Python po lokální kontext jako snímek dat Pandas hello datové rámce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-232">You can plot by using Python code after hello data frame is in local context as a Pandas data frame.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES.
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 <span data-ttu-id="e3e97-233">Hello Spark jádra automaticky vizualizuje výstup hello dotazů SQL (HiveQL), po spuštění kódu hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-233">hello Spark kernel automatically visualizes hello output of SQL (HiveQL) queries after you run hello code.</span></span> <span data-ttu-id="e3e97-234">Můžete si vybrat mezi několik typů vizualizace:</span><span class="sxs-lookup"><span data-stu-id="e3e97-234">You can choose between several types of visualizations:</span></span>

* <span data-ttu-id="e3e97-235">Table</span><span class="sxs-lookup"><span data-stu-id="e3e97-235">Table</span></span>
* <span data-ttu-id="e3e97-236">Výsečový</span><span class="sxs-lookup"><span data-stu-id="e3e97-236">Pie</span></span>
* <span data-ttu-id="e3e97-237">Perokresba</span><span class="sxs-lookup"><span data-stu-id="e3e97-237">Line</span></span>
* <span data-ttu-id="e3e97-238">Oblast</span><span class="sxs-lookup"><span data-stu-id="e3e97-238">Area</span></span>
* <span data-ttu-id="e3e97-239">Panel</span><span class="sxs-lookup"><span data-stu-id="e3e97-239">Bar</span></span>

<span data-ttu-id="e3e97-240">Tady je dat hello tooplot hello kódu:</span><span class="sxs-lookup"><span data-stu-id="e3e97-240">Here's hello code tooplot hello data:</span></span>

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


<span data-ttu-id="e3e97-241">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-241">**Output:**</span></span>

![Tip velikost histogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tip velikost podle velikosti tarif](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a><span data-ttu-id="e3e97-245">Vytvoření funkce transformace funkce a potom připravená data data pro vstup do funkce modelování</span><span class="sxs-lookup"><span data-stu-id="e3e97-245">Create features and transform features, and then prep data for input into modeling functions</span></span>
<span data-ttu-id="e3e97-246">Pro funkce na základě stromu modelování ze Spark ML a MLlib máte s využitím různých technik, jako například přihrádkování, indexování, jeden horkou kódování a vectorization tooprepare cíle a funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-246">For tree-based modeling functions from Spark ML and MLlib, you have tooprepare target and features by using a variety of techniques, such as binning, indexing, one-hot encoding, and vectorization.</span></span> <span data-ttu-id="e3e97-247">Zde jsou toofollow hello postupy v této části:</span><span class="sxs-lookup"><span data-stu-id="e3e97-247">Here are hello procedures toofollow in this section:</span></span>

1. <span data-ttu-id="e3e97-248">Vytvořit novou funkci ve **přihrádkování** čas do provozu časové intervaly.</span><span class="sxs-lookup"><span data-stu-id="e3e97-248">Create a new feature by **binning** hours into traffic time buckets.</span></span>
2. <span data-ttu-id="e3e97-249">Použít **horkou jeden kódování a indexování** toocategorical funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-249">Apply **indexing and one-hot encoding** toocategorical features.</span></span>
3. <span data-ttu-id="e3e97-250">**Ukázka a rozdělení hello datové sady** do učení a testovací zlomků.</span><span class="sxs-lookup"><span data-stu-id="e3e97-250">**Sample and split hello data set** into training and test fractions.</span></span>
4. <span data-ttu-id="e3e97-251">**Zadejte proměnnou školení a funkce**a poté vytvořit indexované nebo horkou jeden kódovaný školení a testování vstupní bod s popiskem odolné distribuovaných datové sady (RDDs) nebo datové rámce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-251">**Specify training variable and features**, and then create indexed or one-hot encoded training and testing input labeled point resilient distributed datasets (RDDs) or data frames.</span></span>
5. <span data-ttu-id="e3e97-252">Automaticky **kategorií a vectorize funkce a cíle** toouse jako vstupy pro modely machine learning.</span><span class="sxs-lookup"><span data-stu-id="e3e97-252">Automatically **categorize and vectorize features and targets** toouse as inputs for machine learning models.</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="e3e97-253">Vytvořit novou funkci přihrádkování čas do provozu čas intervalů</span><span class="sxs-lookup"><span data-stu-id="e3e97-253">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="e3e97-254">Tento kód ukazuje, jak toocreate novou funkci ve přihrádkování čas do provozu čas intervalů a jak toocache hello výsledné datové rámce v paměti.</span><span class="sxs-lookup"><span data-stu-id="e3e97-254">This code shows you how toocreate a new feature by binning hours into traffic time buckets and how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="e3e97-255">Pokud rámce RDDs a data se používají opakovaně, ukládání do mezipaměti vede tooimproved časy spuštění.</span><span class="sxs-lookup"><span data-stu-id="e3e97-255">Where RDDs and data frames are used repeatedly, caching leads tooimproved execution times.</span></span> <span data-ttu-id="e3e97-256">Podle toho budete mezipaměti rámce RDDs a data v několika fázích v hello následující postupy.</span><span class="sxs-lookup"><span data-stu-id="e3e97-256">Accordingly, you'll cache RDDs and data frames at several stages in hello following procedures.</span></span>

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


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a><span data-ttu-id="e3e97-257">Indexování a jeden horkou kódování kategorií funkcí</span><span class="sxs-lookup"><span data-stu-id="e3e97-257">Indexing and one-hot encoding of categorical features</span></span>
<span data-ttu-id="e3e97-258">Hello modelování a předvídání, funkce MLlib potřeba funkce s kategorií vstupní data toobe indexované nebo kódovaný předchozí toouse.</span><span class="sxs-lookup"><span data-stu-id="e3e97-258">hello modeling and predict functions of MLlib require features with categorical input data toobe indexed or encoded prior toouse.</span></span> <span data-ttu-id="e3e97-259">V této části se dozvíte, jak tooindex nebo kódování kategorií funkce pro vstup do funkce modelování hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-259">This section shows you how tooindex or encode categorical features for input into hello modeling functions.</span></span>

<span data-ttu-id="e3e97-260">Třeba tooindex nebo kódování modely různými způsoby v závislosti na modelu hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-260">You need tooindex or encode your models in different ways, depending on hello model.</span></span> <span data-ttu-id="e3e97-261">Například logistic a lineární regrese modely vyžadovat horkou jeden kódování.</span><span class="sxs-lookup"><span data-stu-id="e3e97-261">For example, logistic and linear regression models require one-hot encoding.</span></span> <span data-ttu-id="e3e97-262">Například můžete do tři sloupce funkce rozšířit funkce s tří kategorií.</span><span class="sxs-lookup"><span data-stu-id="e3e97-262">For example, a feature with three categories can be expanded into three feature columns.</span></span> <span data-ttu-id="e3e97-263">Každý sloupec obsahuje 0 nebo 1 v závislosti na kategorii hello pozorování.</span><span class="sxs-lookup"><span data-stu-id="e3e97-263">Each column would contain 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="e3e97-264">MLlib poskytuje hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce pro jeden horkou kódování.</span><span class="sxs-lookup"><span data-stu-id="e3e97-264">MLlib provides hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function for one-hot encoding.</span></span> <span data-ttu-id="e3e97-265">Tato kodér mapuje sloupec popisek indexy tooa sloupce binární vektorů s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-265">This encoder maps a column of label indices tooa column of binary vectors with at most a single one-value.</span></span> <span data-ttu-id="e3e97-266">Toto kódování algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression, může být použité toocategorical funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-266">With this encoding, algorithms that expect numerical valued features, such as logistic regression, can be applied toocategorical features.</span></span>

<span data-ttu-id="e3e97-267">Zde transformace příklady tooshow pouze čtyři proměnné, které jsou řetězce znaků.</span><span class="sxs-lookup"><span data-stu-id="e3e97-267">Here you transform only four variables tooshow examples, which are character strings.</span></span> <span data-ttu-id="e3e97-268">Další proměnné, jako například den v týdnu, reprezentována číselné hodnoty, jako kategorií proměnné také mohou indexu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-268">You also can index other variables, such as weekday, represented by numerical values, as categorical variables.</span></span>

<span data-ttu-id="e3e97-269">Pro indexování, použijte `StringIndexer()`a pro jeden horkou kódování, použijte `OneHotEncoder()` funkce z MLlib.</span><span class="sxs-lookup"><span data-stu-id="e3e97-269">For indexing, use `StringIndexer()`, and for one-hot encoding, use `OneHotEncoder()` functions from MLlib.</span></span> <span data-ttu-id="e3e97-270">Tady je hello tooindex kódu a kódování kategorií funkce:</span><span class="sxs-lookup"><span data-stu-id="e3e97-270">Here is hello code tooindex and encode categorical features:</span></span>

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


<span data-ttu-id="e3e97-271">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-271">**Output:**</span></span>

<span data-ttu-id="e3e97-272">Čas toorun hello buňky: 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="e3e97-272">Time toorun hello cell: 4 seconds.</span></span>

### <a name="sample-and-split-hello-data-set-into-training-and-test-fractions"></a><span data-ttu-id="e3e97-273">Ukázka a rozdělení hello do zlomků učení a testovací datové sady</span><span class="sxs-lookup"><span data-stu-id="e3e97-273">Sample and split hello data set into training and test fractions</span></span>
<span data-ttu-id="e3e97-274">Tento kód vytvoří náhodné vzorky hello dat (v tomto příkladu 25 %).</span><span class="sxs-lookup"><span data-stu-id="e3e97-274">This code creates a random sampling of hello data (25%, in this example).</span></span> <span data-ttu-id="e3e97-275">I když vzorkování není potřeba v tomto příkladu kvůli toohello velikost hello datových sad, hello článek ukazuje, jak můžete zkusit, abyste věděli, jak toouse pro vlastní problémy v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="e3e97-275">Although sampling is not required for this example due toohello size of hello data set, hello article shows you how you can sample so that you know how toouse it for your own problems when needed.</span></span> <span data-ttu-id="e3e97-276">Po velká vzorky lze ušetřit čas významné během cvičení modelů.</span><span class="sxs-lookup"><span data-stu-id="e3e97-276">When samples are large, this can save significant time while you train models.</span></span> <span data-ttu-id="e3e97-277">Ukázka hello dále rozdělením školení část (v tomto příkladu 75 %) a testování částí (v tomto příkladu 25 %) toouse v klasifikaci a regresní modelování.</span><span class="sxs-lookup"><span data-stu-id="e3e97-277">Next, split hello sample into a training part (75%, in this example) and a testing part (25%, in this example) toouse in classification and regression modeling.</span></span>

<span data-ttu-id="e3e97-278">Přidejte řádek tooeach náhodné číslo (mezi 0 a 1) (ve sloupci "rand –"), může být použité tooselect křížové ověření složení během cvičení.</span><span class="sxs-lookup"><span data-stu-id="e3e97-278">Add a random number (between 0 and 1) tooeach row (in a "rand" column) that can be used tooselect cross-validation folds during training.</span></span>

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


<span data-ttu-id="e3e97-279">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-279">**Output:**</span></span>

<span data-ttu-id="e3e97-280">Čas toorun hello buňky: 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="e3e97-280">Time toorun hello cell: 2 seconds.</span></span>

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a><span data-ttu-id="e3e97-281">Zadejte proměnnou školení a funkce a pak vytvořte indexované nebo jeden horkou kódovaný trénování a testování vstup s názvem bez přípony bodu RDDs nebo data rámce</span><span class="sxs-lookup"><span data-stu-id="e3e97-281">Specify training variable and features, and then create indexed or one-hot encoded training and testing input labeled point RDDs or data frames</span></span>
<span data-ttu-id="e3e97-282">Tato část obsahuje kód, který ukazuje, jak tooindex kategorií textová data jako s popiskem typ datového bodu a zakódovat je, abyste mohli používat, tootrain a testování MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="e3e97-282">This section contains code that shows you how tooindex categorical text data as a labeled point data type, and encode it so you can use it tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="e3e97-283">S popiskem bodu objekty jsou RDDs naformátované způsobem, který je nutný jako vstupní data pro většinu v MLlib algoritmů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="e3e97-283">Labeled point objects are RDDs that are formatted in a way that is needed as input data by most of machine learning algorithms in MLlib.</span></span> <span data-ttu-id="e3e97-284">A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.</span><span class="sxs-lookup"><span data-stu-id="e3e97-284">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="e3e97-285">V tento kód zadejte hello Cílová proměnná (závislé) a hello funkce toouse tootrain modelů.</span><span class="sxs-lookup"><span data-stu-id="e3e97-285">In this code, you specify hello target (dependent) variable and hello features toouse tootrain models.</span></span> <span data-ttu-id="e3e97-286">Pak vytvoříte indexované nebo jeden horkou kódovaný trénování a testování vstup s názvem bez přípony bodu RDDs nebo data rámce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-286">Then, you create indexed or one-hot encoded training and testing input labeled point RDDs or data frames.</span></span>

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


<span data-ttu-id="e3e97-287">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-287">**Output:**</span></span>

<span data-ttu-id="e3e97-288">Čas toorun hello buňky: 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="e3e97-288">Time toorun hello cell: 4 seconds.</span></span>

### <a name="automatically-categorize-and-vectorize-features-and-targets-toouse-as-inputs-for-machine-learning-models"></a><span data-ttu-id="e3e97-289">Automaticky kategorií a vectorize funkce a cíle toouse jako vstupy pro modely machine learning</span><span class="sxs-lookup"><span data-stu-id="e3e97-289">Automatically categorize and vectorize features and targets toouse as inputs for machine learning models</span></span>
<span data-ttu-id="e3e97-290">Použijte toouse Spark ML toocategorize hello cíle a funkce na základě stromu modelování funkcí.</span><span class="sxs-lookup"><span data-stu-id="e3e97-290">Use Spark ML toocategorize hello target and features toouse in tree-based modeling functions.</span></span> <span data-ttu-id="e3e97-291">Kód Hello dokončení dvě úlohy:</span><span class="sxs-lookup"><span data-stu-id="e3e97-291">hello code completes two tasks:</span></span>

* <span data-ttu-id="e3e97-292">Vytvoří binární cíl pro klasifikaci pomocí prahovou hodnotu 0,5 přiřazení hodnotu 0 nebo 1 datový bod tooeach mezi 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="e3e97-292">Creates a binary target for classification by assigning a value of 0 or 1 tooeach data point between 0 and 1 by using a threshold value of 0.5.</span></span>
* <span data-ttu-id="e3e97-293">Automaticky rozděluje funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-293">Automatically categorizes features.</span></span> <span data-ttu-id="e3e97-294">Pokud hello počet jedinečných hodnot na číselné u všech funkcí je menší než 32, je zařazený do kategorie této funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-294">If hello number of distinct numerical values for any feature is less than 32, that feature is categorized.</span></span>

<span data-ttu-id="e3e97-295">Tady je hello kód pro tyto dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="e3e97-295">Here's hello code for these two tasks.</span></span>

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



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a><span data-ttu-id="e3e97-296">Model binární klasifikace: předpovědět, zda by měl být placené tip</span><span class="sxs-lookup"><span data-stu-id="e3e97-296">Binary classification model: Predict whether a tip should be paid</span></span>
<span data-ttu-id="e3e97-297">V této části můžete vytvořit tři typy toopredict modely binární klasifikace, jestli by měl být placené tip:</span><span class="sxs-lookup"><span data-stu-id="e3e97-297">In this section, you create three types of binary classification models toopredict whether or not a tip should be paid:</span></span>

* <span data-ttu-id="e3e97-298">A **logistic regresní model** pomocí hello Spark ML `LogisticRegression()` – funkce</span><span class="sxs-lookup"><span data-stu-id="e3e97-298">A **logistic regression model** by using hello Spark ML `LogisticRegression()` function</span></span>
* <span data-ttu-id="e3e97-299">A **model klasifikace náhodných doménové struktury** pomocí hello Spark ML `RandomForestClassifier()` – funkce</span><span class="sxs-lookup"><span data-stu-id="e3e97-299">A **random forest classification model** by using hello Spark ML `RandomForestClassifier()` function</span></span>
* <span data-ttu-id="e3e97-300">A **přechodu zvýšení skóre modelu klasifikace stromu** pomocí hello MLlib `GradientBoostedTrees()` – funkce</span><span class="sxs-lookup"><span data-stu-id="e3e97-300">A **gradient boosting tree classification model** by using hello MLlib `GradientBoostedTrees()` function</span></span>

### <a name="create-a-logistic-regression-model"></a><span data-ttu-id="e3e97-301">Vytvoření modelu logistic regression</span><span class="sxs-lookup"><span data-stu-id="e3e97-301">Create a logistic regression model</span></span>
<span data-ttu-id="e3e97-302">Dále vytvořte logistic regresní model pomocí hello Spark ML `LogisticRegression()` funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-302">Next, create a logistic regression model by using hello Spark ML `LogisticRegression()` function.</span></span> <span data-ttu-id="e3e97-303">Vytvoříte model hello vytváření kódu v sérii kroků:</span><span class="sxs-lookup"><span data-stu-id="e3e97-303">You create hello model building code in a series of steps:</span></span>

1. <span data-ttu-id="e3e97-304">**Train hello model** dat pomocí jednu sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="e3e97-304">**Train hello model** data with one parameter set.</span></span>
2. <span data-ttu-id="e3e97-305">**Vyhodnocení modelu hello** na testovací datové sady s metriky.</span><span class="sxs-lookup"><span data-stu-id="e3e97-305">**Evaluate hello model** on a test data set with metrics.</span></span>
3. <span data-ttu-id="e3e97-306">**Uložit hello model** v úložišti objektů Blob pro budoucí spotřeby.</span><span class="sxs-lookup"><span data-stu-id="e3e97-306">**Save hello model** in Blob storage for future consumption.</span></span>
4. <span data-ttu-id="e3e97-307">**Určení skóre modelu hello** proti testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="e3e97-307">**Score hello model** against test data.</span></span>
5. <span data-ttu-id="e3e97-308">**Vykreslení hello výsledky** s příjemce operační křivek vlastnosti (ROC).</span><span class="sxs-lookup"><span data-stu-id="e3e97-308">**Plot hello results** with receiver operating characteristic (ROC) curves.</span></span>

<span data-ttu-id="e3e97-309">Tady je hello kód pro tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="e3e97-309">Here's hello code for these procedures:</span></span>

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

<span data-ttu-id="e3e97-310">Zatížení, stanovení skóre a uložte výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-310">Load, score, and save hello results.</span></span>

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


<span data-ttu-id="e3e97-311">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-311">**Output:**</span></span>

<span data-ttu-id="e3e97-312">ROC na testovací data = 0.9827381497557599</span><span class="sxs-lookup"><span data-stu-id="e3e97-312">ROC on test data = 0.9827381497557599</span></span>

<span data-ttu-id="e3e97-313">Na místní Pandas dat rámce tooplot hello: křivka ROC použijte jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="e3e97-313">Use Python on local Pandas data frames tooplot hello ROC curve.</span></span>

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


<span data-ttu-id="e3e97-314">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-314">**Output:**</span></span>

![Tip nebo žádné křivka ROC tipu](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a><span data-ttu-id="e3e97-316">Vytvoření modelu klasifikace náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="e3e97-316">Create a random forest classification model</span></span>
<span data-ttu-id="e3e97-317">Dále vytvořte model klasifikace náhodných doménové struktury pomocí hello Spark ML `RandomForestClassifier()` fungovat a pak vyhodnotit hello modelu na testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="e3e97-317">Next, create a random forest classification model by using hello Spark ML `RandomForestClassifier()` function, and then evaluate hello model on test data.</span></span>

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


<span data-ttu-id="e3e97-318">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-318">**Output:**</span></span>

<span data-ttu-id="e3e97-319">ROC na testovací data = 0.9847103571552683</span><span class="sxs-lookup"><span data-stu-id="e3e97-319">ROC on test data = 0.9847103571552683</span></span>

### <a name="create-a-gbt-classification-model"></a><span data-ttu-id="e3e97-320">Vytvoření modelu GBT klasifikace</span><span class="sxs-lookup"><span data-stu-id="e3e97-320">Create a GBT classification model</span></span>
<span data-ttu-id="e3e97-321">Dále vytvořte klasifikaci model GBT pomocí MLlib na `GradientBoostedTrees()` fungovat a pak vyhodnotit hello modelu na testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="e3e97-321">Next, create a GBT classification model by using MLlib's `GradientBoostedTrees()` function, and then evaluate hello model on test data.</span></span>

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


<span data-ttu-id="e3e97-322">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-322">**Output:**</span></span>

<span data-ttu-id="e3e97-323">Oblasti v rámci křivka ROC: 0.9846895479241554</span><span class="sxs-lookup"><span data-stu-id="e3e97-323">Area under ROC curve: 0.9846895479241554</span></span>

## <a name="regression-model-predict-tip-amount"></a><span data-ttu-id="e3e97-324">Regresní model: předpovědi velikost tipu</span><span class="sxs-lookup"><span data-stu-id="e3e97-324">Regression model: Predict tip amount</span></span>
<span data-ttu-id="e3e97-325">V této části vytvoříte dva typy regrese modely toopredict hello tip velikost:</span><span class="sxs-lookup"><span data-stu-id="e3e97-325">In this section, you create two types of regression models toopredict hello tip amount:</span></span>

* <span data-ttu-id="e3e97-326">A **model lineární regrese Vyřešeno** pomocí hello Spark ML `LinearRegression()` funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-326">A **regularized linear regression model** by using hello Spark ML `LinearRegression()` function.</span></span> <span data-ttu-id="e3e97-327">Můžete uložit hello modelu a vyhodnocení modelu hello na testovací data.</span><span class="sxs-lookup"><span data-stu-id="e3e97-327">You'll save hello model and evaluate hello model on test data.</span></span>
* <span data-ttu-id="e3e97-328">A **zvyšovat skóre přechodu stromu regresní model** pomocí hello Spark ML `GBTRegressor()` funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-328">A **gradient-boosting tree regression model** by using hello Spark ML `GBTRegressor()` function.</span></span>

### <a name="create-a-regularized-linear-regression-model"></a><span data-ttu-id="e3e97-329">Vytvořit model lineární regrese Vyřešeno</span><span class="sxs-lookup"><span data-stu-id="e3e97-329">Create a regularized linear regression model</span></span>
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


<span data-ttu-id="e3e97-330">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-330">**Output:**</span></span>

<span data-ttu-id="e3e97-331">Čas toorun hello buňky: 13 sekund.</span><span class="sxs-lookup"><span data-stu-id="e3e97-331">Time toorun hello cell: 13 seconds.</span></span>

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


<span data-ttu-id="e3e97-332">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-332">**Output:**</span></span>

<span data-ttu-id="e3e97-333">R – sqr na testovací data = 0.5960320470835743</span><span class="sxs-lookup"><span data-stu-id="e3e97-333">R-sqr on test data = 0.5960320470835743</span></span>

<span data-ttu-id="e3e97-334">V dalším kroku dotazu hello výsledky testu jako data snímku a jeho použití AutoVizWidget a matplotlib toovisualize ho.</span><span class="sxs-lookup"><span data-stu-id="e3e97-334">Next, query hello test results as a data frame and use AutoVizWidget and matplotlib toovisualize it.</span></span>

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

<span data-ttu-id="e3e97-335">Hello kód vytvoří rámečku místní data z dotazu výstup hello a zobrazuje hello data.</span><span class="sxs-lookup"><span data-stu-id="e3e97-335">hello code creates a local data frame from hello query output and plots hello data.</span></span> <span data-ttu-id="e3e97-336">Hello `%%local` magic vytvoří místní data rámce, `sqlResults`, které můžete použít tooplot s matplotlib.</span><span class="sxs-lookup"><span data-stu-id="e3e97-336">hello `%%local` magic creates a local data frame, `sqlResults`, which you can use tooplot with matplotlib.</span></span>

> [!NOTE]
> <span data-ttu-id="e3e97-337">Tato magic Spark se používá více než jednou. v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e3e97-337">This Spark magic is used multiple times in this article.</span></span> <span data-ttu-id="e3e97-338">Pokud je velká hello množství dat, by měl ukázkové toocreate dat rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="e3e97-338">If hello amount of data is large, you should sample toocreate a data frame that can fit in local memory.</span></span>
> 
> 

<span data-ttu-id="e3e97-339">Vytvořte pozemků pomocí Python matplotlib.</span><span class="sxs-lookup"><span data-stu-id="e3e97-339">Create plots by using Python matplotlib.</span></span>

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

<span data-ttu-id="e3e97-340">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-340">**Output:**</span></span>

![Tip velikost: skutečnost a předpokládaných](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a><span data-ttu-id="e3e97-342">Vytvoření GBT regresní model</span><span class="sxs-lookup"><span data-stu-id="e3e97-342">Create a GBT regression model</span></span>
<span data-ttu-id="e3e97-343">Vytvoření GBT regresní model pomocí hello Spark ML `GBTRegressor()` fungovat a pak vyhodnotit hello modelu na testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="e3e97-343">Create a GBT regression model by using hello Spark ML `GBTRegressor()` function, and then evaluate hello model on test data.</span></span>

<span data-ttu-id="e3e97-344">[Boosted přechodu stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="e3e97-344">[Gradient-boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="e3e97-345">GBTs train rozhodovací stromy interaktivně toominimize funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-345">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="e3e97-346">GBTs můžete použít pro regresní a klasifikace.</span><span class="sxs-lookup"><span data-stu-id="e3e97-346">You can use GBTs for regression and classification.</span></span> <span data-ttu-id="e3e97-347">Se může zpracovat kategorií funkce, nevyžadují funkce škálování a můžete zaznamenat nonlinearities a interakce funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-347">They can handle categorical features, do not require feature scaling, and can capture nonlinearities and feature interactions.</span></span> <span data-ttu-id="e3e97-348">Také můžete je v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="e3e97-348">You also can use them in a multiclass-classification setting.</span></span>

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


<span data-ttu-id="e3e97-349">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-349">**Output:**</span></span>

<span data-ttu-id="e3e97-350">Je test R-sqr: 0.7655383534596654</span><span class="sxs-lookup"><span data-stu-id="e3e97-350">Test R-sqr is: 0.7655383534596654</span></span>

## <a name="advanced-modeling-utilities-for-optimization"></a><span data-ttu-id="e3e97-351">Pokročilé modelování nástrojů pro optimalizaci</span><span class="sxs-lookup"><span data-stu-id="e3e97-351">Advanced modeling utilities for optimization</span></span>
<span data-ttu-id="e3e97-352">V této části použijte nástroje learning počítače, které vývojáři často používají pro optimalizaci modelu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-352">In this section, you use machine learning utilities that developers frequently use for model optimization.</span></span> <span data-ttu-id="e3e97-353">Konkrétně můžete pomocí parametru (vymetání) komínů a křížové ověření optimalizovat modely machine learning třemi různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="e3e97-353">Specifically, you can optimize machine learning models three different ways by using parameter sweeping and cross-validation:</span></span>

* <span data-ttu-id="e3e97-354">Rozdělení dat hello do vlaku a ověření nastaví optimalizovat hello modelu s použitím technologie hyper parametr (vymetání) komínů na trénovací sady a vyhodnocení na sadu ověření (lineární regrese)</span><span class="sxs-lookup"><span data-stu-id="e3e97-354">Split hello data into train and validation sets, optimize hello model by using hyper-parameter sweeping on a training set, and evaluate on a validation set (linear regression)</span></span>
* <span data-ttu-id="e3e97-355">Optimalizovat hello modelu s použitím křížové ověření a technologie hyper parametr komínů pomocí funkce CrossValidator Spark ML (binární klasifikace)</span><span class="sxs-lookup"><span data-stu-id="e3e97-355">Optimize hello model by using cross-validation and hyper-parameter sweeping by using Spark ML's CrossValidator function (binary classification)</span></span>
* <span data-ttu-id="e3e97-356">Optimalizovat hello modelu s použitím vlastní kód křížové ověření a parametr sweeping toouse žádné strojového učení sadu funkce a parametr (lineární regrese)</span><span class="sxs-lookup"><span data-stu-id="e3e97-356">Optimize hello model by using custom cross-validation and parameter-sweeping code toouse any machine learning function and parameter set (linear regression)</span></span>

<span data-ttu-id="e3e97-357">**Křížové ověření** je technika, který vyhodnocuje, jak dobře model trénink na známé sadu dat bude generalize toopredict hello funkce datové sady, na kterých je nebyla cvičena.</span><span class="sxs-lookup"><span data-stu-id="e3e97-357">**Cross-validation** is a technique that assesses how well a model trained on a known set of data will generalize toopredict hello features of data sets on which it has not been trained.</span></span> <span data-ttu-id="e3e97-358">Hello obecnou představu za tato technika je, že cvičení modelu na datové sady známých dat, a pak hello přesnost jeho předpovědi je testován vůči nezávislé datové sady.</span><span class="sxs-lookup"><span data-stu-id="e3e97-358">hello general idea behind this technique is that a model is trained on a data set of known data, and then hello accuracy of its predictions is tested against an independent data set.</span></span> <span data-ttu-id="e3e97-359">Běžná implementace je toodivide datové sady do *tisíc*-složení a potom trénování modelu hello v kruhového dotazování na všechny kromě jednoho hello složení.</span><span class="sxs-lookup"><span data-stu-id="e3e97-359">A common implementation is toodivide a data set into *k*-folds, and then train hello model in a round-robin fashion on all but one of hello folds.</span></span>

<span data-ttu-id="e3e97-360">**Technologie Hyper parametr optimalizace** potížím hello vybrat sadu technologie hyper parametrů pro algoritmu učení, obvykle s cílem hello optimalizace měření výkonu hello algoritmus na nezávislé datové sady.</span><span class="sxs-lookup"><span data-stu-id="e3e97-360">**Hyper-parameter optimization** is hello problem of choosing a set of hyper-parameters for a learning algorithm, usually with hello goal of optimizing a measure of hello algorithm's performance on an independent data set.</span></span> <span data-ttu-id="e3e97-361">Technologie hyper parametr je hodnota, která je nutné zadat mimo hello modelu školení postupu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-361">A hyper-parameter is a value that you must specify outside hello model training procedure.</span></span> <span data-ttu-id="e3e97-362">Předpoklady o technologie hyper parametr hodnoty může ovlivnit hello flexibilitu a přesnosti modelu hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-362">Assumptions about hyper-parameter values can affect hello flexibility and accuracy of hello model.</span></span> <span data-ttu-id="e3e97-363">Rozhodovací stromy mít technologie hyper parametry, například jako hello potřeby hloubkou a počet nechá ve stromu hello.</span><span class="sxs-lookup"><span data-stu-id="e3e97-363">Decision trees have hyper-parameters, for example, such as hello desired depth and number of leaves in hello tree.</span></span> <span data-ttu-id="e3e97-364">Je nutné nastavit termín snížení chybnou klasifikaci pro podporu vektoru počítač (SVM).</span><span class="sxs-lookup"><span data-stu-id="e3e97-364">You must set a misclassification penalty term for a support vector machine (SVM).</span></span>

<span data-ttu-id="e3e97-365">Běžné optimalizace hyper parametr tooperform způsob, jak je toouse vyhledávání mřížky, označované taky jako **oblouku parametr**.</span><span class="sxs-lookup"><span data-stu-id="e3e97-365">A common way tooperform hyper-parameter optimization is toouse a grid search, also called a **parameter sweep**.</span></span> <span data-ttu-id="e3e97-366">V hledání mřížky provádí podrobné prohledávání prostřednictvím hello hodnoty zadané podmnožinu hello hyper parametr místa pro algoritmus učení.</span><span class="sxs-lookup"><span data-stu-id="e3e97-366">In a grid search, an exhaustive search is performed through hello values of a specified subset of hello hyper-parameter space for a learning algorithm.</span></span> <span data-ttu-id="e3e97-367">Křížového ověření může poskytovat metriky toosort out hello optimální výsledky vyprodukované hello mřížky vyhledávacího algoritmu výkonu.</span><span class="sxs-lookup"><span data-stu-id="e3e97-367">Cross-validation can supply a performance metric toosort out hello optimal results produced by hello grid search algorithm.</span></span> <span data-ttu-id="e3e97-368">Pokud používáte křížové ověření (technologie hyper parametr vymetání) komínů, můžete pomoct limit problémy jako overfitting tootraining datový model.</span><span class="sxs-lookup"><span data-stu-id="e3e97-368">If you use cross-validation hyper-parameter sweeping, you can help limit problems like overfitting a model tootraining data.</span></span> <span data-ttu-id="e3e97-369">Tímto způsobem hello modelu zachová hello kapacity tooapply toohello obecné sady dat, ze které hello jste extrahovali Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="e3e97-369">This way, hello model retains hello capacity tooapply toohello general set of data from which hello training data was extracted.</span></span>

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a><span data-ttu-id="e3e97-370">Optimalizace model lineární regrese s hyper parametr (vymetání) komínů</span><span class="sxs-lookup"><span data-stu-id="e3e97-370">Optimize a linear regression model with hyper-parameter sweeping</span></span>
<span data-ttu-id="e3e97-371">Dále rozdělit data do vlaku a ověření sady, použití technologie hyper parametr komínů na školení nastavit toooptimize hello modelu a vyhodnocení na sadu ověření (lineární regrese).</span><span class="sxs-lookup"><span data-stu-id="e3e97-371">Next, split data into train and validation sets, use hyper-parameter sweeping on a training set toooptimize hello model, and evaluate on a validation set (linear regression).</span></span>

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


<span data-ttu-id="e3e97-372">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-372">**Output:**</span></span>

<span data-ttu-id="e3e97-373">Je test R-sqr: 0.6226484708501209</span><span class="sxs-lookup"><span data-stu-id="e3e97-373">Test R-sqr is: 0.6226484708501209</span></span>

### <a name="optimize-hello-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a><span data-ttu-id="e3e97-374">Optimalizovat hello binární klasifikace modelu s použitím (vymetání) křížové ověření a technologie hyper parametr komínů</span><span class="sxs-lookup"><span data-stu-id="e3e97-374">Optimize hello binary classification model by using cross-validation and hyper-parameter sweeping</span></span>
<span data-ttu-id="e3e97-375">Tato část uvádí, jak toooptimize binární klasifikace modelu s použitím sweeping křížové ověření a technologie hyper parametr.</span><span class="sxs-lookup"><span data-stu-id="e3e97-375">This section shows you how toooptimize a binary classification model by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="e3e97-376">Tato služba využívá hello Spark ML `CrossValidator` funkce.</span><span class="sxs-lookup"><span data-stu-id="e3e97-376">This uses hello Spark ML `CrossValidator` function.</span></span>

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


<span data-ttu-id="e3e97-377">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-377">**Output:**</span></span>

<span data-ttu-id="e3e97-378">Čas toorun hello buňky: 33 sekund.</span><span class="sxs-lookup"><span data-stu-id="e3e97-378">Time toorun hello cell: 33 seconds.</span></span>

### <a name="optimize-hello-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a><span data-ttu-id="e3e97-379">Optimalizovat model lineární regrese hello s použitím vlastní křížové ověření a parametr sweeping kód</span><span class="sxs-lookup"><span data-stu-id="e3e97-379">Optimize hello linear regression model by using custom cross-validation and parameter-sweeping code</span></span>
<span data-ttu-id="e3e97-380">V dalším kroku optimalizovat hello modelu s použitím vlastního kódu a identifikovat nejlepší parametry modelu hello pomocí hello kritéria nejvyšší přesnost.</span><span class="sxs-lookup"><span data-stu-id="e3e97-380">Next, optimize hello model by using custom code, and identify hello best model parameters by using hello criterion of highest accuracy.</span></span> <span data-ttu-id="e3e97-381">Pak vytvořte hello konečné modelu, hodnocení hello modelu na testovací data a uložit hello modelu v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e3e97-381">Then, create hello final model, evaluate hello model on test data, and save hello model in Blob storage.</span></span> <span data-ttu-id="e3e97-382">Nakonec načíst hello model, stanovení skóre testovací data a vyhodnotit přesnost.</span><span class="sxs-lookup"><span data-stu-id="e3e97-382">Finally, load hello model, score test data, and evaluate accuracy.</span></span>

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


<span data-ttu-id="e3e97-383">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="e3e97-383">**Output:**</span></span>

<span data-ttu-id="e3e97-384">Čas toorun hello buňky: 61 sekund.</span><span class="sxs-lookup"><span data-stu-id="e3e97-384">Time toorun hello cell: 61 seconds.</span></span>

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a><span data-ttu-id="e3e97-385">Využívat modelů learning vytvořené Spark počítač automaticky pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="e3e97-385">Consume Spark-built machine learning models automatically with Scala</span></span>
<span data-ttu-id="e3e97-386">Přehled témata, která vás provede procesem hello úlohy, které tvoří hello procesu vědecké zpracování dat v Azure najdete v tématu [proces vědecké účely dat Team](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="e3e97-386">For an overview of topics that walk you through hello tasks that comprise hello Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="e3e97-387">[Tým datové vědy proces návody](data-science-process-walkthroughs.md) popisuje další návody začátku do konce, které ukazují hello kroky hello Team datové vědy proces pro konkrétní scénáře.</span><span class="sxs-lookup"><span data-stu-id="e3e97-387">[Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) describes other end-to-end walkthroughs that demonstrate hello steps in hello Team Data Science Process for specific scenarios.</span></span> <span data-ttu-id="e3e97-388">návody Hello také ilustrují, jak toocombine cloudové a místní nástrojů a služeb do pracovního postupu nebo kanálu toocreate inteligentního aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3e97-388">hello walkthroughs also illustrate how toocombine cloud and on-premises tools and services into a workflow or pipeline toocreate an intelligent application.</span></span>

<span data-ttu-id="e3e97-389">[Stanovení skóre modely vytvořené Spark strojové učení](machine-learning-data-science-spark-model-consumption.md) se dozvíte, jak toouse Scala kód tooautomatically načíst a stanovíte jeho skóre nové sady dat s modely machine learning součástí Spark a uloží do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="e3e97-389">[Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) shows you how toouse Scala code tooautomatically load and score new data sets with machine learning models built in Spark and saved in Azure Blob storage.</span></span> <span data-ttu-id="e3e97-390">Můžete podle podle hello pokynů existuje a jednoduše místo hello kód Python Scala kód v tomto článku automatizované spotřeby.</span><span class="sxs-lookup"><span data-stu-id="e3e97-390">You can follow hello instructions provided there, and simply replace hello Python code with Scala code in this article for automated consumption.</span></span>

