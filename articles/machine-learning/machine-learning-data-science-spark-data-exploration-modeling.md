---
title: "Zkoumání dat a modelování pomocí Spark | Microsoft Docs"
description: "Umožňující prezentovat funkce zkoumání a modelování dat sady nástrojů Spark MLlib v Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b989b918-5ba5-4696-b8d0-76ae510a23f4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 711407f7dd9e6d442e3f04a23962487f4808e8e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="data-exploration-and-modeling-with-spark"></a><span data-ttu-id="b042d-103">Zkoumání a modelování dat pomocí Spark</span><span class="sxs-lookup"><span data-stu-id="b042d-103">Data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="b042d-104">Tento návod používá HDInsight Spark pro zkoumání dat a binární klasifikaci a regresní modelování úlohy na vzorku NYC taxi cestě a jízdenky 2013 datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b042d-104">This walkthrough uses HDInsight Spark to do data exploration and binary classification and regression modeling tasks on a sample of the NYC taxi trip and fare 2013 dataset.</span></span>  <span data-ttu-id="b042d-105">Provede vás provede postupem [proces vědecké účely dat](http://aka.ms/datascienceprocess), klient server, pomocí clusteru služby HDInsight Spark pro zpracování a Azure objektů BLOB k ukládání dat a modely.</span><span class="sxs-lookup"><span data-stu-id="b042d-105">It walks you through the steps of the [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs to store the data and the models.</span></span> <span data-ttu-id="b042d-106">Proces jsou zde popsány vizualizuje dat získaných z objektu Blob úložiště Azure a pak připraví dat za účelem vytvoření prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="b042d-106">The process explores and visualizes data brought in from an Azure Storage Blob and then prepares the data to build predictive models.</span></span> <span data-ttu-id="b042d-107">Tyto modely jsou sestavení pomocí sady nástrojů pro Spark MLlib provádění binární klasifikaci a regresní modelování úkolů.</span><span class="sxs-lookup"><span data-stu-id="b042d-107">These models are build using the Spark MLlib toolkit to do binary classification and regression modeling tasks.</span></span>

* <span data-ttu-id="b042d-108">**Binární klasifikace** úloh je k předvídání, zda je tip placené pro cestu.</span><span class="sxs-lookup"><span data-stu-id="b042d-108">The **binary classification** task is to predict whether or not a tip is paid for the trip.</span></span> 
* <span data-ttu-id="b042d-109">**Regrese** úloh je k předvídání množství tip podle dalších funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="b042d-109">The **regression** task is to predict the amount of the tip based on other tip features.</span></span> 

<span data-ttu-id="b042d-110">Modely, které používáme zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromů:</span><span class="sxs-lookup"><span data-stu-id="b042d-110">The models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="b042d-111">[Lineární regrese s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je model lineární regrese, který používá metodu Stochastického přechodu klesání (SGD) a škálování k předvídání objemy tip placené pro optimalizaci a funkce.</span><span class="sxs-lookup"><span data-stu-id="b042d-111">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling to predict the tip amounts paid.</span></span> 
* <span data-ttu-id="b042d-112">[Logistic regression s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) nebo "logit" regression je regresní model, který lze použít při kategorií udělat klasifikace dat je závislé proměnné.</span><span class="sxs-lookup"><span data-stu-id="b042d-112">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when the dependent variable is categorical to do data classification.</span></span> <span data-ttu-id="b042d-113">LBFGS je algoritmus optimalizace jako Newton, blíží algoritmus Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomocí omezené množství paměti počítače a který se často používá v machine learning.</span><span class="sxs-lookup"><span data-stu-id="b042d-113">LBFGS is a quasi-Newton optimization algorithm that approximates the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="b042d-114">[Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="b042d-114">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="b042d-115">Že kombinují mnoho rozhodovacích stromů, aby se snížilo riziko overfitting.</span><span class="sxs-lookup"><span data-stu-id="b042d-115">They combine many decision trees to reduce the risk of overfitting.</span></span> <span data-ttu-id="b042d-116">Náhodné doménovými strukturami se používají pro regresní a klasifikaci a dokáže zpracovat kategorií funkce a lze rozšířit pro nastavení více třídami klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b042d-116">Random forests are used for regression and classification and can handle categorical features and can be extended to the multiclass classification setting.</span></span> <span data-ttu-id="b042d-117">Tyto nevyžadují funkce škálování a mohli zaznamenat nelineárností a funkci interakce.</span><span class="sxs-lookup"><span data-stu-id="b042d-117">They do not require feature scaling and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="b042d-118">Náhodné doménových strukturách jsou jedním z těch nejúspěšnějších strojového učení modely pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="b042d-118">Random forests are one of the most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="b042d-119">[Přechodu boosted stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="b042d-119">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="b042d-120">GBTs cvičení stromů rozhodnutí interaktivně, aby se minimalizoval funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="b042d-120">GBTs train decision trees iteratively to minimize a loss function.</span></span> <span data-ttu-id="b042d-121">GBTs se používají pro regresní a klasifikaci a dokáže zpracovat kategorií funkce, nevyžadují funkce škálování a mohli zaznamenat nelineárností a funkci interakce.</span><span class="sxs-lookup"><span data-stu-id="b042d-121">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="b042d-122">Můžete také používají v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b042d-122">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="b042d-123">Modelování kroky také obsahovat kód znázorňující cvičení, hodnocení a uložit každý typ modelu.</span><span class="sxs-lookup"><span data-stu-id="b042d-123">The modeling steps also contain code showing how to train, evaluate, and save each type of model.</span></span> <span data-ttu-id="b042d-124">Python byla použita k code řešení a zobrazíte relevantní pozemků.</span><span class="sxs-lookup"><span data-stu-id="b042d-124">Python has been used to code the solution and to show the relevant plots.</span></span>   

> [!NOTE]
> <span data-ttu-id="b042d-125">I když toolkit Spark MLlib je navržen pro práci na velkých datových sad, relativně malé ukázkové (pomocí 170 tisíc řádků, přibližně 0,1 % původní datové sady NYC ~ 30 Mb) se zde používá ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="b042d-125">Although the Spark MLlib toolkit is designed to work on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of the original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="b042d-126">Cvičení zadané tady běží efektivně (v přibližně 10 minut) v clusteru HDInsight s 2 uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b042d-126">The exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="b042d-127">Stejný kód, s menšími změnami, můžete použít ke zpracování větší-sady dat, se změny, které pro ukládání do mezipaměti data v paměti a změna velikosti clusteru.</span><span class="sxs-lookup"><span data-stu-id="b042d-127">The same code, with minor modifications, can be used to process larger data-sets, with appropriate modifications for caching data in memory and changing the cluster size.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b042d-128">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b042d-128">Prerequisites</span></span>
<span data-ttu-id="b042d-129">Budete potřebovat účet Azure a Spark 1.6 (nebo Spark 2.0) clusteru HDInsight k dokončení tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="b042d-129">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster to complete this walkthrough.</span></span> <span data-ttu-id="b042d-130">Najdete v článku [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny o tom, jak splnit tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="b042d-130">See the [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how to satisfy these requirements.</span></span> <span data-ttu-id="b042d-131">Toto téma obsahuje také popis NYC 2013 taxíkem data použít se zde a pokyny, jak provést kód z poznámkového bloku Jupyter v clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="b042d-131">That topic also contains a description of the NYC 2013 Taxi data used here and instructions on how to execute code from a Jupyter notebook on the Spark cluster.</span></span> 

## <a name="spark-clusters-and-notebooks"></a><span data-ttu-id="b042d-132">Clustery Spark a poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="b042d-132">Spark clusters and notebooks</span></span>
<span data-ttu-id="b042d-133">Kroky instalace a kódu jsou uvedené v tomto názorném postupu pro používání HDInsight Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="b042d-133">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="b042d-134">Ale poznámkové bloky Jupyter jsou k dispozici pro clustery HDInsight Spark 1.6 a Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="b042d-134">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="b042d-135">Popis poznámkových bloků a odkazy na ně jsou součástí [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) úložiště Githubu, které je obsahují.</span><span class="sxs-lookup"><span data-stu-id="b042d-135">A description of the notebooks and links to them are provided in the [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for the GitHub repository containing them.</span></span> <span data-ttu-id="b042d-136">Kromě toho kód sem a v propojených poznámkových bloků je obecný a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="b042d-136">Moreover, the code here and in the linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="b042d-137">Pokud nepoužíváte HDInsight Spark, může být mírně lišit od co je tady uvedené kroky nastavení a Správa clusteru.</span><span class="sxs-lookup"><span data-stu-id="b042d-137">If you are not using HDInsight Spark, the cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="b042d-138">Pro větší pohodlí si zde jsou uvedeny odkazy na poznámkové bloky Jupyter pro Spark 1.6 (musí být spuštěny v jádra pySpark Poznámkový blok Jupyter serveru) a 2.0 Spark (Chcete-li spustit v jádru pySpark3 Poznámkový blok Jupyter serveru):</span><span class="sxs-lookup"><span data-stu-id="b042d-138">For convenience, here are the links to the Jupyter notebooks for Spark 1.6 (to be run in the pySpark kernel of the Jupyter Notebook server) and  Spark 2.0 (to be run in the pySpark3 kernel of the Jupyter Notebook server):</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="b042d-139">Spark 1.6 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="b042d-139">Spark 1.6 notebooks</span></span>

<span data-ttu-id="b042d-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): poskytuje informace o tom, jak provést zkoumání dat, modelování a vyhodnocování se několik různých algoritmů.</span><span class="sxs-lookup"><span data-stu-id="b042d-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Provides information on how to perform data exploration, modeling, and scoring with several different algorithms.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="b042d-141">Spark 2.0 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="b042d-141">Spark 2.0 notebooks</span></span>
<span data-ttu-id="b042d-142">Regrese a klasifikace úlohy, které jsou implementovány pomocí clusteru Spark 2.0 jsou v samostatných poznámkových bloků a poznámkového bloku klasifikaci používá jinou sadu dat:</span><span class="sxs-lookup"><span data-stu-id="b042d-142">The regression and classification tasks that are implemented using a Spark 2.0 cluster are in separate notebooks and the classification notebook uses a different data set:</span></span>

- <span data-ttu-id="b042d-143">[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Tento soubor obsahuje informace o tom, jak provést zkoumání dat, modelování a vyhodnocování v rámci Spark 2.0 clusterů pomocí cesty NYC taxíkem a tarif sady dat popsané [zde](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="b042d-143">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how to perform data exploration, modeling, and scoring in Spark 2.0 clusters using the NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span> <span data-ttu-id="b042d-144">Tento poznámkový blok, může být to dobrý výchozí bod pro zkoumání rychle kód, který jsme připravili pro Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="b042d-144">This notebook may be a good starting point for quickly exploring the code we have provided for Spark 2.0.</span></span> <span data-ttu-id="b042d-145">Podrobnější Poznámkový blok analyzuje data taxíkem NYC, naleznete další poznámkového bloku v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="b042d-145">For a more detailed notebook analyzes the NYC Taxi data, see the next notebook in this list.</span></span> <span data-ttu-id="b042d-146">Najdete v poznámkách k následující tento seznam porovnávající tyto poznámkových bloků.</span><span class="sxs-lookup"><span data-stu-id="b042d-146">See the notes following this list that compare these notebooks.</span></span> 
- <span data-ttu-id="b042d-147">[Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Tento soubor ukazuje, jak provést data wrangling (Spark SQL a dataframe operations), zkoumání, modelování a vyhodnocování pomocí cesty NYC taxíkem a tarif sady dat popsané [zde](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="b042d-147">[Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): This file shows how to perform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using the NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span>
- <span data-ttu-id="b042d-148">[Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Tento soubor ukazuje, jak provést data wrangling (Spark SQL a dataframe operations), zkoumání, modelování a vyhodnocování pomocí známých letecká společnost na čas odeslání datové sady z 2011 a 2012.</span><span class="sxs-lookup"><span data-stu-id="b042d-148">[Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): This file shows how to perform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using the well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="b042d-149">Jsme integrované letecká společnost datovou sadu s daty počasí letiště (např. větru, teploty, výška atd.) před modelování, takže tyto funkce počasí můžou být součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="b042d-149">We integrated the airline dataset with the airport weather data (e.g. windspeed, temperature, altitude etc.) prior to modeling, so these weather features can be included in the model.</span></span>

<!-- -->

> [!NOTE]
> <span data-ttu-id="b042d-150">Datová sada letecká společnost byl přidán do poznámkových bloků Spark 2.0 abychom vám lépe předvedli použití algoritmů klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b042d-150">The airline dataset was added to the Spark 2.0 notebooks to better illustrate the use of classification algorithms.</span></span> <span data-ttu-id="b042d-151">V následujících tématech informace o letecká společnost v době odeslání datovou sadu a počasí datové sady:</span><span class="sxs-lookup"><span data-stu-id="b042d-151">See the following links for information about airline on-time departure dataset and weather dataset:</span></span>

>- <span data-ttu-id="b042d-152">Letecká společnost na čas odeslání dat: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span><span class="sxs-lookup"><span data-stu-id="b042d-152">Airline on-time departure data: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span></span>

>- <span data-ttu-id="b042d-153">Data o počasí letiště: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span><span class="sxs-lookup"><span data-stu-id="b042d-153">Airport weather data: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span></span> 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
<span data-ttu-id="b042d-154">Poznámkové bloky Spark 2.0 na NYC taxíkem a letecká společnost letu zpoždění-sady dat může trvat 10 minut nebo déle ke spuštění (v závislosti na velikosti vašeho clusteru HDI).</span><span class="sxs-lookup"><span data-stu-id="b042d-154">The Spark 2.0 notebooks on the NYC taxi and airline flight delay data-sets can take 10 mins or more to run (depending on the size of your HDI cluster).</span></span> <span data-ttu-id="b042d-155">První poznámkového bloku v seznamu nahoře zobrazí mnoho aspektů zkoumání dat, vizualizace a ML školení v poznámkovém bloku, který zabere to méně času se spouští s nižší vzorkovat NYC datové sady, ve kterém byly soubory taxíkem a tarif předem připojený k modelu: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) tento poznámkový blok trvá mnohem kratší dobu dokončit (v minutách 2-3) a může být dobrou výchozí bod pro rychle prohlížení kódu uvádíme Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="b042d-155">The first notebook in the above list shows many aspects of the data exploration, visualization and ML model training in a notebook that takes less time to run with down-sampled NYC data set, in which the taxi and fare files have been pre-joined: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) This notebook takes a much shorter time to finish (2-3 mins) and may be a good starting point for quickly exploring the code we have provided for Spark 2.0.</span></span> 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
<span data-ttu-id="b042d-156">Níže jsou související s použitím Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="b042d-156">The descriptions below are related to using Spark 1.6.</span></span> <span data-ttu-id="b042d-157">Verze Spark 2.0 použijte poznámkových bloků popsané a uvedený výše.</span><span class="sxs-lookup"><span data-stu-id="b042d-157">For Spark 2.0 versions, please use the notebooks described and linked above.</span></span> 

<!-- -->

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a><span data-ttu-id="b042d-158">Instalace: umístění úložiště, knihovny a kontext přednastavené Spark</span><span class="sxs-lookup"><span data-stu-id="b042d-158">Setup: storage locations, libraries, and the preset Spark context</span></span>
<span data-ttu-id="b042d-159">Spark se bude moct číst a zapisovat do Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="b042d-159">Spark is able to read and write to Azure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="b042d-160">Takže existující data uložená existuje může zpracovat pomocí Spark a výsledky uložené v WASB znovu.</span><span class="sxs-lookup"><span data-stu-id="b042d-160">So any of your existing data stored there can be processed using Spark and the results stored again in WASB.</span></span>

<span data-ttu-id="b042d-161">Cesta k uložení modely nebo souborů v WASB, je třeba zadat správně.</span><span class="sxs-lookup"><span data-stu-id="b042d-161">To save models or files in WASB, the path needs to be specified properly.</span></span> <span data-ttu-id="b042d-162">Výchozí kontejner, který je připojen ke clusteru Spark se může odkazovat pomocí cesty od verze: "wasb: / / /".</span><span class="sxs-lookup"><span data-stu-id="b042d-162">The default container attached to the Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="b042d-163">Odkazují jiné umístění "wasb: / /".</span><span class="sxs-lookup"><span data-stu-id="b042d-163">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="b042d-164">Nastavení cesty adresáře pro umístění úložiště v WASB</span><span class="sxs-lookup"><span data-stu-id="b042d-164">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="b042d-165">Následující příklad kódu určuje umístění dat ke čtení a cesty k adresáři modelu úložiště, kde je uložen výstupní modelu:</span><span class="sxs-lookup"><span data-stu-id="b042d-165">The following code sample specifies the location of the data to be read and the path for the model storage directory to which the model output is saved:</span></span>

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a><span data-ttu-id="b042d-166">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="b042d-166">Import libraries</span></span>
<span data-ttu-id="b042d-167">Nastavení taky vyžaduje import potřebné knihovny.</span><span class="sxs-lookup"><span data-stu-id="b042d-167">Set up also requires importing necessary libraries.</span></span> <span data-ttu-id="b042d-168">Nastavit kontext spark a importovat potřebné knihovny s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b042d-168">Set spark context and import necessary libraries with the following code:</span></span>

    # IMPORT LIBRARIES
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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="b042d-169">Předvolby kontextu Spark a Magic PySpark</span><span class="sxs-lookup"><span data-stu-id="b042d-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="b042d-170">Jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu.</span><span class="sxs-lookup"><span data-stu-id="b042d-170">The PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="b042d-171">Proto není potřeba nastavit Spark nebo vývoji Hive kontexty explicitně před zahájením práce s aplikací.</span><span class="sxs-lookup"><span data-stu-id="b042d-171">So you do not need to set the Spark or Hive contexts explicitly before you start working with the application you are developing.</span></span> <span data-ttu-id="b042d-172">Tyto kontexty jsou dostupné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b042d-172">These contexts are available for you by default.</span></span> <span data-ttu-id="b042d-173">Tyto kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="b042d-173">These contexts are:</span></span>

* <span data-ttu-id="b042d-174">sc - pro Spark</span><span class="sxs-lookup"><span data-stu-id="b042d-174">sc - for Spark</span></span> 
* <span data-ttu-id="b042d-175">sqlContext - pro Hive</span><span class="sxs-lookup"><span data-stu-id="b042d-175">sqlContext - for Hive</span></span>

<span data-ttu-id="b042d-176">Poskytuje jádra PySpark některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%.</span><span class="sxs-lookup"><span data-stu-id="b042d-176">The PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="b042d-177">Existují dva takové příkazy, které se používají v tyto ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="b042d-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="b042d-178">**%% místní** Určuje, že kód v dalších řádcích se má provádět místně.</span><span class="sxs-lookup"><span data-stu-id="b042d-178">**%%local** Specifies that the code in subsequent lines is to be executed locally.</span></span> <span data-ttu-id="b042d-179">Kód musí být platný kód Python.</span><span class="sxs-lookup"><span data-stu-id="b042d-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="b042d-180">**%% sql -o <variable name>**  provede dotaz Hive proti sqlContext.</span><span class="sxs-lookup"><span data-stu-id="b042d-180">**%%sql -o <variable name>** Executes a Hive query against the sqlContext.</span></span> <span data-ttu-id="b042d-181">Pokud je předán parametr -o, výsledek dotazu je uchován v %% lokální kontext Python jako Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="b042d-181">If the -o parameter is passed, the result of the query is persisted in the %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="b042d-182">Pro další informace o jádrech pro poznámkové bloky Jupyter a předdefinovanou "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="b042d-182">For more information on the kernels for Jupyter notebooks and the predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="b042d-183">Přijímání dat z veřejného objektu blob</span><span class="sxs-lookup"><span data-stu-id="b042d-183">Data ingestion from public blob</span></span>
<span data-ttu-id="b042d-184">Prvním krokem v procesu vědecké účely dat je ingestují data, která má být analyzován z zdrojů kde je umístěn do vašeho prostředí zkoumání a modelování data.</span><span class="sxs-lookup"><span data-stu-id="b042d-184">The first step in the data science process is to ingest the data to be analyzed from sources where is resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="b042d-185">Prostředí je Spark v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="b042d-185">The environment is Spark in this walkthrough.</span></span> <span data-ttu-id="b042d-186">Tato část obsahuje kód pro dokončení řadu úloh:</span><span class="sxs-lookup"><span data-stu-id="b042d-186">This section contains the code to complete a series of tasks:</span></span>

* <span data-ttu-id="b042d-187">ingestování vzorek dat modelovat</span><span class="sxs-lookup"><span data-stu-id="b042d-187">ingest the data sample to be modeled</span></span>
* <span data-ttu-id="b042d-188">čtení ve vstupní datové sady (uložený jako soubor TSV)</span><span class="sxs-lookup"><span data-stu-id="b042d-188">read in the input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="b042d-189">formátování a vyčištění dat.</span><span class="sxs-lookup"><span data-stu-id="b042d-189">format and clean the data</span></span>
* <span data-ttu-id="b042d-190">vytvářet a ukládat do mezipaměti objektů (RDDs nebo datových rámců) v paměti</span><span class="sxs-lookup"><span data-stu-id="b042d-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="b042d-191">Zaregistrujte se jako dočasné tabulky v kontextu SQL.</span><span class="sxs-lookup"><span data-stu-id="b042d-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="b042d-192">Zde je kód pro přijímat data.</span><span class="sxs-lookup"><span data-stu-id="b042d-192">Here is the code for data ingestion.</span></span>

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
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


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="b042d-193">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-193">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-194">Doba k provedení výše buňky: 51.72 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-194">Time taken to execute above cell: 51.72 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="b042d-195">Zkoumání dat a vizualizaci</span><span class="sxs-lookup"><span data-stu-id="b042d-195">Data exploration & visualization</span></span>
<span data-ttu-id="b042d-196">Jakmile data vstoupila v Spark, je dalším krokem v procesu vědecké účely dat získali lepší představu o dat přes zkoumání a vizualizace.</span><span class="sxs-lookup"><span data-stu-id="b042d-196">Once the data has been brought into Spark, the next step in the data science process is to gain deeper understanding of the data through exploration and visualization.</span></span> <span data-ttu-id="b042d-197">V této části jsme zkontrolujte taxíkem dat pomocí dotazů SQL a vykreslení cílových proměnných a potenciální funkcí pro visual kontroly.</span><span class="sxs-lookup"><span data-stu-id="b042d-197">In this section, we examine the taxi data using SQL queries and plot the target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="b042d-198">Konkrétně jsme vykreslení četnost počty osobní v taxi cest, frekvenci tip objemu a jak se typy liší podle částka platby a typu.</span><span class="sxs-lookup"><span data-stu-id="b042d-198">Specifically, we plot the frequency of passenger counts in taxi trips, the frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a><span data-ttu-id="b042d-199">Vykreslení histogram frekvencí počet osobní v ukázce taxíkem cest</span><span class="sxs-lookup"><span data-stu-id="b042d-199">Plot a histogram of passenger count frequencies in the sample of taxi trips</span></span>
<span data-ttu-id="b042d-200">Tento kód a následné fragmenty použijte k dotazování na ukázkové a místní magic k vykreslení dat SQL magic.</span><span class="sxs-lookup"><span data-stu-id="b042d-200">This code and subsequent snippets use SQL magic to query the sample and local magic to plot the data.</span></span>

* <span data-ttu-id="b042d-201">**SQL magic (`%%sql`)** jádra PySpark HDInsight podporuje snadno vložené HiveQL dotazy proti sqlContext.</span><span class="sxs-lookup"><span data-stu-id="b042d-201">**SQL magic (`%%sql`)** The HDInsight PySpark kernel supports easy inline HiveQL queries against the sqlContext.</span></span> <span data-ttu-id="b042d-202">(-O VARIABLE_NAME) argument potrvají výstup příkazu jazyka SQL jako Pandas DataFrame na serveru Jupyter.</span><span class="sxs-lookup"><span data-stu-id="b042d-202">The (-o VARIABLE_NAME) argument persists the output of the SQL query as a Pandas DataFrame on the Jupyter server.</span></span> <span data-ttu-id="b042d-203">To znamená, že je k dispozici v místním režimu.</span><span class="sxs-lookup"><span data-stu-id="b042d-203">This means it is available in the local mode.</span></span>
* <span data-ttu-id="b042d-204"> **`%%local` Magic** slouží ke spouštění kódu místně na serveru Jupyter, což je headnode clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b042d-204">The **`%%local` magic** is used to run code locally on the Jupyter server, which is the headnode of the HDInsight cluster.</span></span> <span data-ttu-id="b042d-205">Obvykle použijete, `%%local` magic ve spojení s `%%sql` magic s parametrem -o.</span><span class="sxs-lookup"><span data-stu-id="b042d-205">Typically, you use `%%local` magic in conjunction with the `%%sql` magic with -o parameter.</span></span> <span data-ttu-id="b042d-206">Parametr -o by zachovat výstup příkazu jazyka SQL místně a pak %% místní magic by aktivovat další sadu fragment kódu ke spouštění místně na výstupu dotazů SQL, který je místně trvalé</span><span class="sxs-lookup"><span data-stu-id="b042d-206">The -o parameter would persist the output of the SQL query locally and then %%local magic would trigger the next set of code snippet to run locally against the output of the SQL queries that is persisted locally</span></span>

<span data-ttu-id="b042d-207">Výstup se automaticky vizualizuje po spuštění kódu.</span><span class="sxs-lookup"><span data-stu-id="b042d-207">The output is automatically visualized after you run the code.</span></span>

<span data-ttu-id="b042d-208">Tento dotaz načte služebních cest podle počtu osobní.</span><span class="sxs-lookup"><span data-stu-id="b042d-208">This query retrieves the trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

<span data-ttu-id="b042d-209">Tento kód vytvoří místní data snímku z výstupu dotazu a ukazuje zeměpisný data.</span><span class="sxs-lookup"><span data-stu-id="b042d-209">This code creates a local data-frame from the query output and plots the data.</span></span> <span data-ttu-id="b042d-210">`%%local` Magic vytvoří místní rámce dat, `sqlResults`, který může být použit pro vykreslení s matplotlib.</span><span class="sxs-lookup"><span data-stu-id="b042d-210">The `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="b042d-211">Tato PySpark magic se používá více než jednou. v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="b042d-211">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="b042d-212">Pokud je velké množství dat, by měl ukázkové k vytvoření data rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="b042d-212">If the amount of data is large, you should sample to create a data-frame that can fit in local memory.</span></span>
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="b042d-213">Zde je kód k vykreslení služebních cest dle počtů osobní</span><span class="sxs-lookup"><span data-stu-id="b042d-213">Here is the code to plot the trips by passenger counts</span></span>

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

<span data-ttu-id="b042d-214">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-214">**OUTPUT:**</span></span>

![Frekvence cestě podle počtu osobní](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

<span data-ttu-id="b042d-216">Můžete vybrat mezi několika různých typů vizualizace (tabulky, kruhový, řádku, oblasti nebo panelu) pomocí **typ** tlačítka nabídky v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="b042d-216">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using the **Type** menu buttons in the notebook.</span></span> <span data-ttu-id="b042d-217">Vykreslení panelu se zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="b042d-217">The Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="b042d-218">Vykreslení histogram tip objemy a jak se liší podle počtu a tarif objemy osobní tip velikost.</span><span class="sxs-lookup"><span data-stu-id="b042d-218">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="b042d-219">Pomocí příkazu jazyka SQL ukázková data.</span><span class="sxs-lookup"><span data-stu-id="b042d-219">Use a SQL query to sample data.</span></span>

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST THE sqlContext
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


<span data-ttu-id="b042d-220">Tuto buňku kód používá k vytvoření tři pozemků data dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="b042d-220">This code cell uses the SQL query to create three plots the data.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


<span data-ttu-id="b042d-221">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-221">**OUTPUT:**</span></span> 

![Tip rozdělení částky](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip velikost podle velikosti tarif](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="b042d-225">Funkce analýzy, transformaci a data přípravy pro modelování</span><span class="sxs-lookup"><span data-stu-id="b042d-225">Feature engineering, transformation and data preparation for modeling</span></span>
<span data-ttu-id="b042d-226">Tato část popisuje a poskytuje kód pro postupy, které slouží k přípravě dat pro použití v ML modelování.</span><span class="sxs-lookup"><span data-stu-id="b042d-226">This section describes and provides the code for procedures used to prepare data for use in ML modeling.</span></span> <span data-ttu-id="b042d-227">Ukazuje, jak provést následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="b042d-227">It shows how to do the following tasks:</span></span>

* <span data-ttu-id="b042d-228">Vytvořit novou funkci přihrádkování čas do provozu čas intervalů</span><span class="sxs-lookup"><span data-stu-id="b042d-228">Create a new feature by binning hours into traffic time buckets</span></span>
* <span data-ttu-id="b042d-229">Index a kódování kategorií funkce</span><span class="sxs-lookup"><span data-stu-id="b042d-229">Index and encode categorical features</span></span>
* <span data-ttu-id="b042d-230">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="b042d-230">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="b042d-231">Vytvořit náhodné dílčí vzorkování dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="b042d-231">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
* <span data-ttu-id="b042d-232">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="b042d-232">Feature scaling</span></span>
* <span data-ttu-id="b042d-233">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="b042d-233">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="b042d-234">Vytvořit novou funkci přihrádkování čas do provozu čas intervalů</span><span class="sxs-lookup"><span data-stu-id="b042d-234">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="b042d-235">Tento kód ukazuje postup vytvořit novou funkci přihrádkování čas do kbelíků čas provoz a výsledné datové rámce v paměti do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="b042d-235">This code shows how to create a new feature by binning hours into traffic time buckets and then how to cache the resulting data frame in memory.</span></span> <span data-ttu-id="b042d-236">Pokud se opakovaně používají odolné distribuované datové sady (RDDs) a datových rámců, ukládání do mezipaměti vede k lepší časy spuštění.</span><span class="sxs-lookup"><span data-stu-id="b042d-236">Where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly, caching leads to improved execution times.</span></span> <span data-ttu-id="b042d-237">Podle toho jsme mezipaměti RDDs a datové rámce v několika fázích v návodu.</span><span class="sxs-lookup"><span data-stu-id="b042d-237">Accordingly, we cache RDDs and data-frames at several stages in the walkthrough.</span></span> 

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
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

<span data-ttu-id="b042d-238">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-238">**OUTPUT:**</span></span> 

<span data-ttu-id="b042d-239">126050</span><span class="sxs-lookup"><span data-stu-id="b042d-239">126050</span></span>

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a><span data-ttu-id="b042d-240">Index a kódování kategorií funkce pro vstup do funkce modelování</span><span class="sxs-lookup"><span data-stu-id="b042d-240">Index and encode categorical features for input into modeling functions</span></span>
<span data-ttu-id="b042d-241">V této části ukazuje, jak index nebo kódování kategorií funkce pro vstup do funkce modelování.</span><span class="sxs-lookup"><span data-stu-id="b042d-241">This section shows how to index or encode categorical features for input into the modeling functions.</span></span> <span data-ttu-id="b042d-242">Modelování a předvídání funkce MLlib vyžadují funkce s kategorií vstupní data na indexované nebo kódovaný před použití.</span><span class="sxs-lookup"><span data-stu-id="b042d-242">The modeling and predict functions of MLlib require features with categorical input data to be indexed or encoded prior to use.</span></span> <span data-ttu-id="b042d-243">V závislosti na modelu budete muset index nebo zakódovat je různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="b042d-243">Depending on the model, you need to index or encode them in different ways:</span></span>  

* <span data-ttu-id="b042d-244">**Na základě stromu modelování** vyžaduje kategorií kódovaný jako číselné hodnoty (například funkce s tří kategorií může být kódován 0, 1, 2).</span><span class="sxs-lookup"><span data-stu-id="b042d-244">**Tree-based modeling** requires categories to be encoded as numerical values (for example, a feature with three categories may be encoded with 0, 1, 2).</span></span> <span data-ttu-id="b042d-245">To zajišťuje na MLlib [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) funkce.</span><span class="sxs-lookup"><span data-stu-id="b042d-245">This is provided by MLlib’s [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) function.</span></span> <span data-ttu-id="b042d-246">Tato funkce kóduje sloupec řetězce popisků ke sloupci popisek indexy, které jsou seřazené podle četnosti popisek.</span><span class="sxs-lookup"><span data-stu-id="b042d-246">This function encodes a string column of labels to a column of label indices that are ordered by label frequencies.</span></span> <span data-ttu-id="b042d-247">I když indexované číselné hodnoty vstupu a manipulaci s daty, k je považovat za správně kategorií lze zadat algoritmů na základě stromu.</span><span class="sxs-lookup"><span data-stu-id="b042d-247">Although indexed with numerical values for input and data handling, the tree-based algorithms can be specified to treat them appropriately as categories.</span></span> 
* <span data-ttu-id="b042d-248">**Modely Logistic a lineární regrese** vyžadují jeden horkou kódování, kde, například funkce s tří kategorií lze rozšířit na tři sloupce funkce, s každou obsahující 0 nebo 1 v závislosti na kategorii pozorování.</span><span class="sxs-lookup"><span data-stu-id="b042d-248">**Logistic and Linear Regression models** require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on the category of an observation.</span></span> <span data-ttu-id="b042d-249">Poskytuje MLlib [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce udělat za provozu jeden kódování.</span><span class="sxs-lookup"><span data-stu-id="b042d-249">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function to do one-hot encoding.</span></span> <span data-ttu-id="b042d-250">Tato kodér mapuje sloupec popisek indexů ke sloupci binárního vektory, s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b042d-250">This encoder maps a column of label indices to a column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="b042d-251">Toto kódování umožňuje algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression, má být použita pro kategorií funkce.</span><span class="sxs-lookup"><span data-stu-id="b042d-251">This encoding allows algorithms that expect numerical valued features, such as logistic regression, to be applied to categorical features.</span></span>

<span data-ttu-id="b042d-252">Tady je kód pro index a kódování kategorií funkce:</span><span class="sxs-lookup"><span data-stu-id="b042d-252">Here is the code to index and encode categorical features:</span></span>

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-253">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-253">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-254">Doba k provedení výše buňky: 1,28 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-254">Time taken to execute above cell: 1.28 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="b042d-255">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="b042d-255">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="b042d-256">Tato část obsahuje kód, který ukazuje, jak index kategorií textová data jako datový typ s popiskem bodu a jeho kódování, aby se může použít k trénování a testování MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b042d-256">This section contains code that shows how to index categorical text data as a labeled point data type and encode it so that it can be used to train and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="b042d-257">S popiskem bodu objekty jsou odolné distribuované datové sady (RDD) formátu způsobem, který je nutný jako vstupní data pro většinu ML algoritmy v MLlib.</span><span class="sxs-lookup"><span data-stu-id="b042d-257">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="b042d-258">A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.</span><span class="sxs-lookup"><span data-stu-id="b042d-258">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>  

<span data-ttu-id="b042d-259">Tato část obsahuje kód, který ukazuje, jak index kategorií textová data jako [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) datový typ a zakódovat je, aby se může použít k trénování a testování MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b042d-259">This section contains code that shows how to index categorical text data as a [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) data type and encode it so that it can be used to train and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="b042d-260">S popiskem bodu objekty jsou odolné distribuované datové sady (RDD) skládající se z funkce vector a popisku (cíl a odpověď proměnné).</span><span class="sxs-lookup"><span data-stu-id="b042d-260">Labeled point objects are Resilient Distributed Datasets (RDD) consisting of a label (target/response variable) and feature vector.</span></span> <span data-ttu-id="b042d-261">Tento formát je potřeba jako vstup mnoho algoritmy ML v MLlib.</span><span class="sxs-lookup"><span data-stu-id="b042d-261">This format is needed as input by many ML algorithms in MLlib.</span></span>

<span data-ttu-id="b042d-262">Tady je kód pro index a kódování textu funkce pro binární klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="b042d-262">Here is the code to index and encode text features for binary classification.</span></span>

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


<span data-ttu-id="b042d-263">Tady je kód ke kódování a indexu kategorií text funkce pro analýzu lineární regrese.</span><span class="sxs-lookup"><span data-stu-id="b042d-263">Here is the code to encode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="b042d-264">Vytvořit náhodné dílčí vzorkování dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="b042d-264">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
<span data-ttu-id="b042d-265">Tento kód vytvoří náhodné vzorky dat (25 % tady slouží).</span><span class="sxs-lookup"><span data-stu-id="b042d-265">This code creates a random sampling of the data (25% is used here).</span></span> <span data-ttu-id="b042d-266">I když to není nutné v tomto příkladu kvůli překročení velikosti datové sady, ukážeme, jak můžete vzorkovat tady, víte, jak používat pro váš vlastní problém v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b042d-266">Although it is not required for this example due to the size of the dataset, we demonstrate how you can sample here so you know how to use it for your own problem when needed.</span></span> <span data-ttu-id="b042d-267">Po velká vzorky lze ušetřit čas významné při školení modelů.</span><span class="sxs-lookup"><span data-stu-id="b042d-267">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="b042d-268">Další jsme rozdělit vzorku na školení část (v tomto poli 75 %) a testování částí (zde 25 %) pro použití v klasifikaci a regresní modelování.</span><span class="sxs-lookup"><span data-stu-id="b042d-268">Next we split the sample into a training part (75% here) and a testing part (25% here) to use in classification and regression modeling.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-269">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-269">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-270">Doba k provedení nad buňku: 0,24 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-270">Time taken to execute above cell: 0.24 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="b042d-271">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="b042d-271">Feature scaling</span></span>
<span data-ttu-id="b042d-272">Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle.</span><span class="sxs-lookup"><span data-stu-id="b042d-272">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in the objective function.</span></span> <span data-ttu-id="b042d-273">Kód pro funkci škálování používá [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) funkce, které chcete odchylku jednotky škálování.</span><span class="sxs-lookup"><span data-stu-id="b042d-273">The code for feature scaling uses the [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) to scale the features to unit variance.</span></span> <span data-ttu-id="b042d-274">Pochází od MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD), oblíbených algoritmus pro trénování širokou škálu jiných modelů strojového učení například Vyřešeno regresí nebo support vector počítače (SVM).</span><span class="sxs-lookup"><span data-stu-id="b042d-274">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>

> [!NOTE]
> <span data-ttu-id="b042d-275">Našli jsme algoritmus LinearRegressionWithSGD být citlivé funkce škálování.</span><span class="sxs-lookup"><span data-stu-id="b042d-275">We have found the LinearRegressionWithSGD algorithm to be sensitive to feature scaling.</span></span>
> 
> 

<span data-ttu-id="b042d-276">Tady je kód, který škálování proměnné pro použití s regularized lineární SGD algoritmus.</span><span class="sxs-lookup"><span data-stu-id="b042d-276">Here is the code to scale variables for use with the regularized linear SGD algorithm.</span></span>

    # FEATURE SCALING

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-277">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-277">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-278">Doba k provedení výše buňky: 13.17 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-278">Time taken to execute above cell: 13.17 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="b042d-279">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="b042d-279">Cache objects in memory</span></span>
<span data-ttu-id="b042d-280">Ukládání do mezipaměti na vstupní data rámce objekty používá pro klasifikaci, regrese a rozšířit funkce může snížit čas potřebný pro trénování a testování ML algoritmů.</span><span class="sxs-lookup"><span data-stu-id="b042d-280">The time taken for training and testing of ML algorithms can be reduced by caching the input data frame objects used for classification, regression, and scaled features.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-281">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-281">**OUTPUT:**</span></span> 

<span data-ttu-id="b042d-282">Doba k provedení nad buňku: 0,15 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-282">Time taken to execute above cell: 0.15 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="b042d-283">Předpovědět, zda je tip placené s modely binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="b042d-283">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="b042d-284">Tato část uvádí, jak použít tři modely pro predikci úlohy binární klasifikace zda tip je placené taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="b042d-284">This section shows how use three models for the binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="b042d-285">Modely uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="b042d-285">The models presented are:</span></span>

* <span data-ttu-id="b042d-286">Vyřešeno logistic regression</span><span class="sxs-lookup"><span data-stu-id="b042d-286">Regularized logistic regression</span></span> 
* <span data-ttu-id="b042d-287">Model náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b042d-287">Random forest model</span></span>
* <span data-ttu-id="b042d-288">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="b042d-288">Gradient Boosting Trees</span></span>

<span data-ttu-id="b042d-289">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="b042d-289">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="b042d-290">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="b042d-290">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="b042d-291">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="b042d-291">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="b042d-292">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="b042d-292">**Saving model** in blob for future consumption</span></span>

### <a name="classification-using-logistic-regression"></a><span data-ttu-id="b042d-293">Klasifikace pomocí logistic regression</span><span class="sxs-lookup"><span data-stu-id="b042d-293">Classification using logistic regression</span></span>
<span data-ttu-id="b042d-294">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b042d-294">The code in this section shows how to train, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

<span data-ttu-id="b042d-295">**Trénování modelu logistic regression pomocí odchylka nákladů a hyperparameter (vymetání) komínů**</span><span class="sxs-lookup"><span data-stu-id="b042d-295">**Train the logistic regression model using CV and hyperparameter sweeping**</span></span>

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics


    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="b042d-296">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-296">**OUTPUT:**</span></span> 

<span data-ttu-id="b042d-297">Koeficienty: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="b042d-297">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="b042d-298">Zachycení:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="b042d-298">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="b042d-299">Doba k provedení výše buňky: 14.43 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-299">Time taken to execute above cell: 14.43 seconds</span></span>

<span data-ttu-id="b042d-300">**Vyhodnocení modelu binární klasifikace s standardní metriky**</span><span class="sxs-lookup"><span data-stu-id="b042d-300">**Evaluate the binary classification model with standard metrics**</span></span>

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))

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


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="b042d-301">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-301">**OUTPUT:**</span></span> 

<span data-ttu-id="b042d-302">Oblasti v rámci PR = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="b042d-302">Area under PR = 0.985297691373</span></span>

<span data-ttu-id="b042d-303">Oblasti v rámci ROC = 0.983714670256</span><span class="sxs-lookup"><span data-stu-id="b042d-303">Area under ROC = 0.983714670256</span></span>

<span data-ttu-id="b042d-304">Souhrnné statistiky</span><span class="sxs-lookup"><span data-stu-id="b042d-304">Summary Stats</span></span>

<span data-ttu-id="b042d-305">Přesnost = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="b042d-305">Precision = 0.984304060189</span></span>

<span data-ttu-id="b042d-306">Odvolat = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="b042d-306">Recall = 0.984304060189</span></span>

<span data-ttu-id="b042d-307">F1 Stanovení skóre = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="b042d-307">F1 Score = 0.984304060189</span></span>

<span data-ttu-id="b042d-308">Doba k provedení výše buňky: 57.61 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-308">Time taken to execute above cell: 57.61 seconds</span></span>

<span data-ttu-id="b042d-309">**Vykreslení křivka ROC.**</span><span class="sxs-lookup"><span data-stu-id="b042d-309">**Plot the ROC curve.**</span></span>

<span data-ttu-id="b042d-310">*PredictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku.</span><span class="sxs-lookup"><span data-stu-id="b042d-310">The *predictionAndLabelsDF* is registered as a table, *tmp_results*, in the previous cell.</span></span> <span data-ttu-id="b042d-311">*tmp_results* slouží k proveďte dotazy a výstup výsledků do sqlResults dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="b042d-311">*tmp_results* can be used to do queries and output results into the sqlResults data-frame for plotting.</span></span> <span data-ttu-id="b042d-312">Zde je kód.</span><span class="sxs-lookup"><span data-stu-id="b042d-312">Here is the code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="b042d-313">Tady je kód vykreslení křivka ROC a provádět předpovědi.</span><span class="sxs-lookup"><span data-stu-id="b042d-313">Here is the code to make predictions and plot the ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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


<span data-ttu-id="b042d-314">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-314">**OUTPUT:**</span></span>

![Logistic regression ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="b042d-316">Klasifikace náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b042d-316">Random forest classification</span></span>
<span data-ttu-id="b042d-317">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit model náhodných doménové struktury, který předpovídá, zda je tip zaplacení cesty v NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b042d-317">The code in this section shows how to train, evaluate, and save a random forest model that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

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
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-318">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-318">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-319">Oblasti v rámci ROC = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="b042d-319">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="b042d-320">Doba k provedení výše buňky: 31.09 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-320">Time taken to execute above cell: 31.09 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="b042d-321">Přechodu zvýšení skóre klasifikace stromů</span><span class="sxs-lookup"><span data-stu-id="b042d-321">Gradient boosting trees classification</span></span>
<span data-ttu-id="b042d-322">Kód v této části ukazuje, jak cvičení, vyhodnotit a uložte přechodu zvýšení skóre stromy model, který předpovídá, zda je pro cesty v cestě taxíkem NYC placené tip a jízdenky datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b042d-322">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="b042d-323">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-323">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-324">Oblasti v rámci ROC = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="b042d-324">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="b042d-325">Doba k provedení výše buňky: 19.76 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-325">Time taken to execute above cell: 19.76 seconds</span></span>

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a><span data-ttu-id="b042d-326">Předpověď objemy tip pro služebních cest taxíkem s modely regrese</span><span class="sxs-lookup"><span data-stu-id="b042d-326">Predict tip amounts for taxi trips with regression models</span></span>
<span data-ttu-id="b042d-327">Tato část uvádí, jak použít tři modely pro predikci množství tip úlohy regrese placené taxíkem cesty na základě jiných funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="b042d-327">This section shows how use three models for the regression task of predicting the amount of the tip paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="b042d-328">Modely uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="b042d-328">The models presented are:</span></span>

* <span data-ttu-id="b042d-329">Vyřešeno lineární regrese</span><span class="sxs-lookup"><span data-stu-id="b042d-329">Regularized linear regression</span></span>
* <span data-ttu-id="b042d-330">Náhodné doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b042d-330">Random forest</span></span>
* <span data-ttu-id="b042d-331">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="b042d-331">Gradient Boosting Trees</span></span>

<span data-ttu-id="b042d-332">Tyto modely byly popsané v úvodu.</span><span class="sxs-lookup"><span data-stu-id="b042d-332">These models were described in the introduction.</span></span> <span data-ttu-id="b042d-333">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="b042d-333">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="b042d-334">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="b042d-334">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="b042d-335">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="b042d-335">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="b042d-336">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="b042d-336">**Saving model** in blob for future consumption</span></span>

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="b042d-337">Lineární regrese s SGD</span><span class="sxs-lookup"><span data-stu-id="b042d-337">Linear regression with SGD</span></span>
<span data-ttu-id="b042d-338">Kód v této části ukazuje, jak používat škálovat funkce ke cvičení lineární regrese, který stochastického přechodu klesání (SGD) používá pro optimalizaci a jak stanovení skóre, hodnocení a uložit model v Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="b042d-338">The code in this section shows how to use scaled features to train a linear regression that uses stochastic gradient descent (SGD) for optimization, and how to score, evaluate, and save the model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="b042d-339">V našich zkušeností může být problémy s konvergence LinearRegressionWithSGD modelů a parametry musí být optimalizované pečlivě pro získání platný model jejich změnit.</span><span class="sxs-lookup"><span data-stu-id="b042d-339">In our experience, there can be issues with the convergence of LinearRegressionWithSGD models, and parameters need to be changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="b042d-340">Škálování proměnných výrazně pomáhá s konvergence.</span><span class="sxs-lookup"><span data-stu-id="b042d-340">Scaling of variables significantly helps with convergence.</span></span> 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-341">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-341">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-342">Koeficienty: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]</span><span class="sxs-lookup"><span data-stu-id="b042d-342">Coefficients: [0.00457675809917, -0.0226314167349, -0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981, -0.000987181489428, -0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995, -0.00990211159703, -0.00637410344522, 0.545083566179, -0.536756072402, 0.0105762393099, -0.0130117577055, 0.0129304737772, -0.00171065945959]</span></span>

<span data-ttu-id="b042d-343">Zachytávat: 0.853872718283</span><span class="sxs-lookup"><span data-stu-id="b042d-343">Intercept: 0.853872718283</span></span>

<span data-ttu-id="b042d-344">RMSE = 1.24190115863</span><span class="sxs-lookup"><span data-stu-id="b042d-344">RMSE = 1.24190115863</span></span>

<span data-ttu-id="b042d-345">R sqr = 0.608017146081</span><span class="sxs-lookup"><span data-stu-id="b042d-345">R-sqr = 0.608017146081</span></span>

<span data-ttu-id="b042d-346">Doba k provedení výše buňky: 58.42 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-346">Time taken to execute above cell: 58.42 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="b042d-347">Regrese náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b042d-347">Random Forest regression</span></span>
<span data-ttu-id="b042d-348">Kód v této části ukazuje, jak cvičení, hodnocení a uložit regrese náhodných doménové struktury, který předpovídá velikost tip pro data NYC taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="b042d-348">The code in this section shows how to train, evaluate, and save a random forest regression that predicts tip amount for the NYC taxi trip data.</span></span>

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-349">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-349">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-350">RMSE = 0.891209218139</span><span class="sxs-lookup"><span data-stu-id="b042d-350">RMSE = 0.891209218139</span></span>

<span data-ttu-id="b042d-351">R sqr = 0.759661334921</span><span class="sxs-lookup"><span data-stu-id="b042d-351">R-sqr = 0.759661334921</span></span>

<span data-ttu-id="b042d-352">Doba k provedení výše buňky: 49.21 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-352">Time taken to execute above cell: 49.21 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="b042d-353">Přechodu zvýšení skóre regresní stromy</span><span class="sxs-lookup"><span data-stu-id="b042d-353">Gradient boosting trees regression</span></span>
<span data-ttu-id="b042d-354">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá velikost tip pro data NYC taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="b042d-354">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts tip amount for the NYC taxi trip data.</span></span>

<span data-ttu-id="b042d-355">** Natrénování a vyhodnocení **</span><span class="sxs-lookup"><span data-stu-id="b042d-355">**Train and evaluate **</span></span>

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b042d-356">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-356">**OUTPUT:**</span></span>

<span data-ttu-id="b042d-357">RMSE = 0.908473148639</span><span class="sxs-lookup"><span data-stu-id="b042d-357">RMSE = 0.908473148639</span></span>

<span data-ttu-id="b042d-358">R sqr = 0.753835096681</span><span class="sxs-lookup"><span data-stu-id="b042d-358">R-sqr = 0.753835096681</span></span>

<span data-ttu-id="b042d-359">Doba k provedení výše buňky: 34.52 sekund</span><span class="sxs-lookup"><span data-stu-id="b042d-359">Time taken to execute above cell: 34.52 seconds</span></span>

<span data-ttu-id="b042d-360">**Vykreslení.**</span><span class="sxs-lookup"><span data-stu-id="b042d-360">**Plot**</span></span>

<span data-ttu-id="b042d-361">*tmp_results* je registrován jako tabulku Hive v předchozí buňku.</span><span class="sxs-lookup"><span data-stu-id="b042d-361">*tmp_results* is registered as a Hive table in the previous cell.</span></span> <span data-ttu-id="b042d-362">Výsledky z tabulky se výstup do *sqlResults* dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="b042d-362">Results from the table are output into the *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="b042d-363">Zde je kód</span><span class="sxs-lookup"><span data-stu-id="b042d-363">Here is the code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

<span data-ttu-id="b042d-364">Tady je kód k vykreslení data s využitím serveru Jupyter.</span><span class="sxs-lookup"><span data-stu-id="b042d-364">Here is the code to plot the data using the Jupyter server.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)


<span data-ttu-id="b042d-365">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b042d-365">**OUTPUT:**</span></span>

![Skutečný vs předpovědět tip objemy](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a><span data-ttu-id="b042d-367">Vyčištění objektů z paměti</span><span class="sxs-lookup"><span data-stu-id="b042d-367">Clean up objects from memory</span></span>
<span data-ttu-id="b042d-368">Použití `unpersist()` odstranit objekty uložené v mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="b042d-368">Use `unpersist()` to delete objects cached in memory.</span></span>

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()

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


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a><span data-ttu-id="b042d-369">Umístění úložiště záznam modely pro využívání a vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="b042d-369">Record storage locations of the models for consumption and scoring</span></span>
<span data-ttu-id="b042d-370">Spotřebovat a stanovení skóre nezávislé datové sadě služby popsané v [skóre a vyhodnocení modelů vytvořené Spark strojové učení](machine-learning-data-science-spark-model-consumption.md) tématu, musíte zkopírovat a vložit tyto názvy souborů obsahující uložené modely vytvořená zde do poznámkového bloku Jupyter spotřeby.</span><span class="sxs-lookup"><span data-stu-id="b042d-370">To consume and score an independent dataset described in the [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) topic, you need to copy and paste these file names containing the saved models created here into the Consumption Jupyter notebook.</span></span> <span data-ttu-id="b042d-371">Tady je kód, který vytiskněte cesty k souborům modelu, potřebujete existuje.</span><span class="sxs-lookup"><span data-stu-id="b042d-371">Here is the code to print out the paths to model files you need there.</span></span>

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="b042d-372">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="b042d-372">**OUTPUT**</span></span>

<span data-ttu-id="b042d-373">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"</span><span class="sxs-lookup"><span data-stu-id="b042d-373">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"</span></span>

<span data-ttu-id="b042d-374">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"</span><span class="sxs-lookup"><span data-stu-id="b042d-374">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"</span></span>

<span data-ttu-id="b042d-375">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"</span><span class="sxs-lookup"><span data-stu-id="b042d-375">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"</span></span>

<span data-ttu-id="b042d-376">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"</span><span class="sxs-lookup"><span data-stu-id="b042d-376">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"</span></span>

<span data-ttu-id="b042d-377">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"</span><span class="sxs-lookup"><span data-stu-id="b042d-377">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"</span></span>

<span data-ttu-id="b042d-378">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"</span><span class="sxs-lookup"><span data-stu-id="b042d-378">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"</span></span>

## <a name="whats-next"></a><span data-ttu-id="b042d-379">Co dále?</span><span class="sxs-lookup"><span data-stu-id="b042d-379">What's next?</span></span>
<span data-ttu-id="b042d-380">Teď, když jste vytvořili regrese a klasifikace modely s Spark MlLib, jste připraveni se dozvíte, jak pro stanovení skóre a vyhodnocení těchto modelů.</span><span class="sxs-lookup"><span data-stu-id="b042d-380">Now that you have created regression and classification models with the Spark MlLib, you are ready to learn how to score and evaluate these models.</span></span> <span data-ttu-id="b042d-381">Zkoumání pokročilé dat a modelování poznámkového bloku dives hlubší do včetně křížového ověřování, technologie hyper parametr komínů a vyhodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="b042d-381">The advanced data exploration and modeling notebook dives deeper into including cross-validation, hyper-parameter sweeping, and model evaluation.</span></span> 

<span data-ttu-id="b042d-382">**Model spotřeby:** další postup stanovení skóre a vyhodnocení modelů klasifikace a regrese vytvořené v tomto tématu najdete v tématu [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="b042d-382">**Model consumption:** To learn how to score and evaluate the classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

<span data-ttu-id="b042d-383">**Křížové ověření a hyperparameter (vymetání) komínů**: najdete v části [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí (vymetání) křížové ověření a technologie hyper parametr komínů</span><span class="sxs-lookup"><span data-stu-id="b042d-383">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping</span></span>

