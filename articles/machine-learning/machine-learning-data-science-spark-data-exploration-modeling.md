---
title: "zkoumání aaaData a modelování s Spark | Microsoft Docs"
description: "Server Showcase hello zkoumání dat a modelování možnosti sady nástrojů hello Spark MLlib v Azure."
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
ms.openlocfilehash: cf5cee4575053f5954b08ca659dfc39c53798371
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-exploration-and-modeling-with-spark"></a><span data-ttu-id="b5898-103">Zkoumání a modelování dat pomocí Spark</span><span class="sxs-lookup"><span data-stu-id="b5898-103">Data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="b5898-104">Tento návod používá zkoumání dat toodo HDInsight Spark a binární klasifikaci a regresní modelování úlohy na vzorku hello NYC taxi cestě a jízdenky 2013 datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b5898-104">This walkthrough uses HDInsight Spark toodo data exploration and binary classification and regression modeling tasks on a sample of hello NYC taxi trip and fare 2013 dataset.</span></span>  <span data-ttu-id="b5898-105">Ho vás provede kroky hello hello [proces vědecké účely dat](http://aka.ms/datascienceprocess), klient server, pomocí HDInsight Spark clusteru pro zpracování a toostore hello dat a hello modely objektů Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="b5898-105">It walks you through hello steps of hello [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs toostore hello data and hello models.</span></span> <span data-ttu-id="b5898-106">proces Hello jsou zde popsány vizualizuje dat získaných z objektu Blob úložiště Azure a pak připraví hello data toobuild prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="b5898-106">hello process explores and visualizes data brought in from an Azure Storage Blob and then prepares hello data toobuild predictive models.</span></span> <span data-ttu-id="b5898-107">Tyto modely jsou sestavení pomocí hello Spark MLlib toolkit toodo binární klasifikace a regresní modelování úlohy.</span><span class="sxs-lookup"><span data-stu-id="b5898-107">These models are build using hello Spark MLlib toolkit toodo binary classification and regression modeling tasks.</span></span>

* <span data-ttu-id="b5898-108">Hello **binární klasifikace** úloha je toopredict, zda je pro cestu hello placené tip.</span><span class="sxs-lookup"><span data-stu-id="b5898-108">hello **binary classification** task is toopredict whether or not a tip is paid for hello trip.</span></span> 
* <span data-ttu-id="b5898-109">Hello **regrese** úloh je toopredict hello množství hello tip podle dalších funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="b5898-109">hello **regression** task is toopredict hello amount of hello tip based on other tip features.</span></span> 

<span data-ttu-id="b5898-110">Hello modely, které používáme zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromů:</span><span class="sxs-lookup"><span data-stu-id="b5898-110">hello models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="b5898-111">[Lineární regrese s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je model lineární regrese, který používá metodu Stochastického přechodu klesání (SGD) a škálování toopredict hello tip objemy placené pro optimalizaci a funkce.</span><span class="sxs-lookup"><span data-stu-id="b5898-111">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling toopredict hello tip amounts paid.</span></span> 
* <span data-ttu-id="b5898-112">[Logistic regression s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) nebo "logit" regression je regresní model, který lze použít při hello závislé proměnné je klasifikace dat kategorií toodo.</span><span class="sxs-lookup"><span data-stu-id="b5898-112">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when hello dependent variable is categorical toodo data classification.</span></span> <span data-ttu-id="b5898-113">LBFGS je algoritmus optimalizace jako Newton, blíží algoritmus hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomocí omezené množství paměti počítače a který se často používá v machine learning.</span><span class="sxs-lookup"><span data-stu-id="b5898-113">LBFGS is a quasi-Newton optimization algorithm that approximates hello Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="b5898-114">[Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="b5898-114">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="b5898-115">Že kombinují mnoho rozhodovací stromy tooreduce hello riziko overfitting.</span><span class="sxs-lookup"><span data-stu-id="b5898-115">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="b5898-116">Náhodné doménové struktury se používají pro regresní a klasifikace a dokáže zpracovat kategorií funkce a je možné rozšířit toohello více třídami klasifikace nastavení.</span><span class="sxs-lookup"><span data-stu-id="b5898-116">Random forests are used for regression and classification and can handle categorical features and can be extended toohello multiclass classification setting.</span></span> <span data-ttu-id="b5898-117">Funkce škálování a jsou možné toocapture nelineárností a funkce interakce se nevyžadují.</span><span class="sxs-lookup"><span data-stu-id="b5898-117">They do not require feature scaling and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="b5898-118">Náhodné doménových strukturách jsou jedním z hello těch nejúspěšnějších modelů strojového učení pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="b5898-118">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="b5898-119">[Přechodu boosted stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="b5898-119">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="b5898-120">GBTs train rozhodovací stromy interaktivně toominimize funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="b5898-120">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="b5898-121">GBTs se používají pro regresní a klasifikace a dokáže zpracovat kategorií funkce, nevyžadují funkce škálování a mít toocapture nelineárností a funkce, interakce.</span><span class="sxs-lookup"><span data-stu-id="b5898-121">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="b5898-122">Můžete také používají v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b5898-122">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="b5898-123">Hello modelování kroky také obsahovat kódu, který ukazuje, jak tootrain, hodnocení a uložit každý typ modelu.</span><span class="sxs-lookup"><span data-stu-id="b5898-123">hello modeling steps also contain code showing how tootrain, evaluate, and save each type of model.</span></span> <span data-ttu-id="b5898-124">Python byl použité toocode hello řešení a tooshow hello relevantní pozemků.</span><span class="sxs-lookup"><span data-stu-id="b5898-124">Python has been used toocode hello solution and tooshow hello relevant plots.</span></span>   

> [!NOTE]
> <span data-ttu-id="b5898-125">Sice hello Spark MLlib toolkit navrženou toowork v rozsáhlých datových sad, relativně malé ukázkové (pomocí 170 tisíc řádků, přibližně 0,1 % hello původní datové sady NYC ~ 30 Mb) se zde používá ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="b5898-125">Although hello Spark MLlib toolkit is designed toowork on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of hello original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="b5898-126">cvičení Hello zadané tady běží efektivně (v přibližně 10 minut) v clusteru HDInsight s 2 uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="b5898-126">hello exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="b5898-127">Hello stejný kód s méně závažné změny lze použít tooprocess větší-sady dat, se změny, které pro ukládání do mezipaměti data v paměti a změna velikosti clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-127">hello same code, with minor modifications, can be used tooprocess larger data-sets, with appropriate modifications for caching data in memory and changing hello cluster size.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b5898-128">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5898-128">Prerequisites</span></span>
<span data-ttu-id="b5898-129">Budete potřebovat účet Azure a Spark 1.6 (nebo Spark 2.0) cluster HDInsight toocomplete tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="b5898-129">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster toocomplete this walkthrough.</span></span> <span data-ttu-id="b5898-130">V tématu hello [přehled o vědecké zpracování dat pomocí Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md) pokyny toosatisfy tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="b5898-130">See hello [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how toosatisfy these requirements.</span></span> <span data-ttu-id="b5898-131">Toto téma obsahuje také popis hello NYC 2013 taxíkem dat použít se zde a pokyny, jak tooexecute kód z poznámkového bloku Jupyter v clusteru Spark hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-131">That topic also contains a description of hello NYC 2013 Taxi data used here and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster.</span></span> 

## <a name="spark-clusters-and-notebooks"></a><span data-ttu-id="b5898-132">Clustery Spark a poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="b5898-132">Spark clusters and notebooks</span></span>
<span data-ttu-id="b5898-133">Kroky instalace a kódu jsou uvedené v tomto názorném postupu pro používání HDInsight Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="b5898-133">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="b5898-134">Ale poznámkové bloky Jupyter jsou k dispozici pro clustery HDInsight Spark 1.6 a Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="b5898-134">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="b5898-135">Popis toothem hello poznámkových bloků a odkazy jsou uvedeny v hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) hello Githubu úložiště, které je obsahují.</span><span class="sxs-lookup"><span data-stu-id="b5898-135">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="b5898-136">Kromě toho hello kód sem a v poznámkových bloků hello propojené je obecný a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="b5898-136">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="b5898-137">Pokud nepoužíváte HDInsight Spark, může být kroky instalace a správy clusteru hello mírně lišit od co se zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="b5898-137">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="b5898-138">Pro usnadnění práce uvádíme hello odkazy toohello poznámkové bloky Jupyter pro Spark 1.6 (toobe spustit v jádra pySpark hello Dobrý den Poznámkový blok Jupyter serveru) a Spark 2.0 (toobe spustit v hello pySpark3 jádra systému hello Poznámkový blok Jupyter serveru):</span><span class="sxs-lookup"><span data-stu-id="b5898-138">For convenience, here are hello links toohello Jupyter notebooks for Spark 1.6 (toobe run in hello pySpark kernel of hello Jupyter Notebook server) and  Spark 2.0 (toobe run in hello pySpark3 kernel of hello Jupyter Notebook server):</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="b5898-139">Spark 1.6 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="b5898-139">Spark 1.6 notebooks</span></span>

<span data-ttu-id="b5898-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): obsahuje informace o tooperform zkoumání dat, modelování a vyhodnocování se několik různých algoritmů.</span><span class="sxs-lookup"><span data-stu-id="b5898-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Provides information on how tooperform data exploration, modeling, and scoring with several different algorithms.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="b5898-141">Spark 2.0 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="b5898-141">Spark 2.0 notebooks</span></span>
<span data-ttu-id="b5898-142">Hello regrese a klasifikace úlohy, které jsou implementovány pomocí clusteru Spark 2.0 jsou v samostatných poznámkových bloků a poznámkového bloku klasifikace hello používá jinou sadu dat:</span><span class="sxs-lookup"><span data-stu-id="b5898-142">hello regression and classification tasks that are implemented using a Spark 2.0 cluster are in separate notebooks and hello classification notebook uses a different data set:</span></span>

- <span data-ttu-id="b5898-143">[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Tento soubor obsahuje informace o jak tooperform zkoumání dat, modelování a vyhodnocování v rámci Spark 2.0 clusterů pomocí hello NYC taxíkem cesty tarif popsané na sadu dat a [zde](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="b5898-143">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how tooperform data exploration, modeling, and scoring in Spark 2.0 clusters using hello NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span> <span data-ttu-id="b5898-144">Tento poznámkový blok, může být to dobrý výchozí bod pro zkoumání rychle hello kódu, které uvádíme Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="b5898-144">This notebook may be a good starting point for quickly exploring hello code we have provided for Spark 2.0.</span></span> <span data-ttu-id="b5898-145">Podrobnější Poznámkový blok analyzuje hello NYC taxíkem data, najdete v části Další poznámkového bloku hello v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="b5898-145">For a more detailed notebook analyzes hello NYC Taxi data, see hello next notebook in this list.</span></span> <span data-ttu-id="b5898-146">Naleznete v poznámkách hello následující tohoto seznamu, které porovnávají tyto poznámkových bloků.</span><span class="sxs-lookup"><span data-stu-id="b5898-146">See hello notes following this list that compare these notebooks.</span></span> 
- <span data-ttu-id="b5898-147">[Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Tento soubor ukazuje, jak tooperform data wrangling (Spark SQL a dataframe operations), zkoumání, modelování a vyhodnocování pomocí hello NYC taxíkem služební cestě a tarif sady dat popsané [ Zde](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="b5898-147">[Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): This file shows how tooperform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using hello NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span>
- <span data-ttu-id="b5898-148">[Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Tento soubor ukazuje, jak tooperform data wrangling (Spark SQL a dataframe operations), zkoumání, modelování a vyhodnocování pomocí hello dobře známé letecká společnost na čas odeslání datové sady z 2011 a 2012.</span><span class="sxs-lookup"><span data-stu-id="b5898-148">[Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): This file shows how tooperform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using hello well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="b5898-149">Datová sada letecká společnost hello jsme integrované s hello letiště počasí data (např. větru, teploty, výška atd.) toomodeling předchozí, tak tyto funkce počasí můžou být součástí modelu hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-149">We integrated hello airline dataset with hello airport weather data (e.g. windspeed, temperature, altitude etc.) prior toomodeling, so these weather features can be included in hello model.</span></span>

<!-- -->

> [!NOTE]
> <span data-ttu-id="b5898-150">Datová sada letecká společnost Hello byl přidán poznámkových bloků toohello Spark 2.0 toobetter ilustrují použití hello algoritmů klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b5898-150">hello airline dataset was added toohello Spark 2.0 notebooks toobetter illustrate hello use of classification algorithms.</span></span> <span data-ttu-id="b5898-151">Viz následující odkazy na informace o datové sady na čas odeslání letecká společnost a datovou sadu počasí hello:</span><span class="sxs-lookup"><span data-stu-id="b5898-151">See hello following links for information about airline on-time departure dataset and weather dataset:</span></span>

>- <span data-ttu-id="b5898-152">Letecká společnost na čas odeslání dat: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span><span class="sxs-lookup"><span data-stu-id="b5898-152">Airline on-time departure data: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span></span>

>- <span data-ttu-id="b5898-153">Data o počasí letiště: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span><span class="sxs-lookup"><span data-stu-id="b5898-153">Airport weather data: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span></span> 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
<span data-ttu-id="b5898-154">Hello poznámkových bloků Spark 2.0 na hello NYC taxíkem a letecká společnost letu zpoždění-sady dat může trvat 10 minut nebo další toorun (v závislosti na velikosti hello clusteru HDI).</span><span class="sxs-lookup"><span data-stu-id="b5898-154">hello Spark 2.0 notebooks on hello NYC taxi and airline flight delay data-sets can take 10 mins or more toorun (depending on hello size of your HDI cluster).</span></span> <span data-ttu-id="b5898-155">Hello první poznámkového bloku v hello výše seznamu zobrazí mnoho aspektů hello zkoumání dat, vizualizace a ML školení v poznámkovém bloku, která přebírá méně času toorun s nižší vzorkovat NYC datové sady, ve které hello taxíkem a tarif soubory byly předem připojený k modelu: [ Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) trvá tento poznámkový blok mnohem kratší dobu toofinish (v minutách 2-3) a může být dobrou výchozí bod pro zkoumání rychle hello kód máme k dispozici pro Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="b5898-155">hello first notebook in hello above list shows many aspects of hello data exploration, visualization and ML model training in a notebook that takes less time toorun with down-sampled NYC data set, in which hello taxi and fare files have been pre-joined: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) This notebook takes a much shorter time toofinish (2-3 mins) and may be a good starting point for quickly exploring hello code we have provided for Spark 2.0.</span></span> 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
<span data-ttu-id="b5898-156">popisy Hello níže jsou související toousing Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="b5898-156">hello descriptions below are related toousing Spark 1.6.</span></span> <span data-ttu-id="b5898-157">Verze Spark 2.0 použijte poznámkových bloků hello popsané a uvedený výše.</span><span class="sxs-lookup"><span data-stu-id="b5898-157">For Spark 2.0 versions, please use hello notebooks described and linked above.</span></span> 

<!-- -->

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="b5898-158">Instalační program: umístění úložiště, knihovny a hello přednastavení Spark kontextu</span><span class="sxs-lookup"><span data-stu-id="b5898-158">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="b5898-159">Spark je možné tooAzure tooread a zápisu objektu Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="b5898-159">Spark is able tooread and write tooAzure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="b5898-160">Takže existující data v ní uloženy může zpracovat pomocí Spark a hello výsledky uložené v znovu WASB.</span><span class="sxs-lookup"><span data-stu-id="b5898-160">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="b5898-161">modely toosave či soubory v WASB, musí cesta hello toobe zadán správně.</span><span class="sxs-lookup"><span data-stu-id="b5898-161">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="b5898-162">Hello clusteru Spark toohello výchozí kontejner připojené můžete odkazovat pomocí cesty počínaje: "wasb: / / /".</span><span class="sxs-lookup"><span data-stu-id="b5898-162">hello default container attached toohello Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="b5898-163">Odkazují jiné umístění "wasb: / /".</span><span class="sxs-lookup"><span data-stu-id="b5898-163">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="b5898-164">Nastavení cesty adresáře pro umístění úložiště v WASB</span><span class="sxs-lookup"><span data-stu-id="b5898-164">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="b5898-165">Hello následující ukázka kódu určuje hello umístění toobe hello data přečíst a uložena hello cesta pro model výstup hello directory toowhich hello modelu úložiště:</span><span class="sxs-lookup"><span data-stu-id="b5898-165">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved:</span></span>

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a><span data-ttu-id="b5898-166">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="b5898-166">Import libraries</span></span>
<span data-ttu-id="b5898-167">Nastavení taky vyžaduje import potřebné knihovny.</span><span class="sxs-lookup"><span data-stu-id="b5898-167">Set up also requires importing necessary libraries.</span></span> <span data-ttu-id="b5898-168">Nastavit kontext spark a importovat potřebné knihovny s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="b5898-168">Set spark context and import necessary libraries with hello following code:</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="b5898-169">Předvolby kontextu Spark a Magic PySpark</span><span class="sxs-lookup"><span data-stu-id="b5898-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="b5898-170">Hello jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu.</span><span class="sxs-lookup"><span data-stu-id="b5898-170">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="b5898-171">Proto není nutné tooset kontexty Spark nebo Hive hello explicitně před zahájením práce s hello aplikací, které vyvíjíte.</span><span class="sxs-lookup"><span data-stu-id="b5898-171">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="b5898-172">Tyto kontexty jsou dostupné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b5898-172">These contexts are available for you by default.</span></span> <span data-ttu-id="b5898-173">Tyto kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="b5898-173">These contexts are:</span></span>

* <span data-ttu-id="b5898-174">sc - pro Spark</span><span class="sxs-lookup"><span data-stu-id="b5898-174">sc - for Spark</span></span> 
* <span data-ttu-id="b5898-175">sqlContext - pro Hive</span><span class="sxs-lookup"><span data-stu-id="b5898-175">sqlContext - for Hive</span></span>

<span data-ttu-id="b5898-176">Hello jádra PySpark poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%.</span><span class="sxs-lookup"><span data-stu-id="b5898-176">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="b5898-177">Existují dva takové příkazy, které se používají v tyto ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="b5898-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="b5898-178">**%% místní** Určuje, že je kód hello v dalších řádcích toobe provést místně.</span><span class="sxs-lookup"><span data-stu-id="b5898-178">**%%local** Specifies that hello code in subsequent lines is toobe executed locally.</span></span> <span data-ttu-id="b5898-179">Kód musí být platný kód Python.</span><span class="sxs-lookup"><span data-stu-id="b5898-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="b5898-180">**%% sql -o <variable name>**  provede dotaz Hive proti hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="b5898-180">**%%sql -o <variable name>** Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="b5898-181">Pokud není předán parametr -o hello hello výsledek dotazu hello je uchován v hello %% lokální kontext Python jako Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="b5898-181">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="b5898-182">Pro další informace o hello jádra pro poznámkové bloky Jupyter a hello předdefinované "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="b5898-182">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="b5898-183">Přijímání dat z veřejného objektu blob</span><span class="sxs-lookup"><span data-stu-id="b5898-183">Data ingestion from public blob</span></span>
<span data-ttu-id="b5898-184">Hello prvním krokem v procesu vědecké účely dat hello je tooingest hello data toobe analyzovány z zdrojů kde je umístěn do vašeho prostředí zkoumání a modelování data.</span><span class="sxs-lookup"><span data-stu-id="b5898-184">hello first step in hello data science process is tooingest hello data toobe analyzed from sources where is resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="b5898-185">Hello prostředí je Spark v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="b5898-185">hello environment is Spark in this walkthrough.</span></span> <span data-ttu-id="b5898-186">Tato část obsahuje hello kód toocomplete řadu úloh:</span><span class="sxs-lookup"><span data-stu-id="b5898-186">This section contains hello code toocomplete a series of tasks:</span></span>

* <span data-ttu-id="b5898-187">ingestování toobe ukázková data hello modelován</span><span class="sxs-lookup"><span data-stu-id="b5898-187">ingest hello data sample toobe modeled</span></span>
* <span data-ttu-id="b5898-188">Přečtěte si hello vstupní datové sady (uložený jako soubor TSV)</span><span class="sxs-lookup"><span data-stu-id="b5898-188">read in hello input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="b5898-189">Formát a vyčištění hello data</span><span class="sxs-lookup"><span data-stu-id="b5898-189">format and clean hello data</span></span>
* <span data-ttu-id="b5898-190">vytvářet a ukládat do mezipaměti objektů (RDDs nebo datových rámců) v paměti</span><span class="sxs-lookup"><span data-stu-id="b5898-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="b5898-191">Zaregistrujte se jako dočasné tabulky v kontextu SQL.</span><span class="sxs-lookup"><span data-stu-id="b5898-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="b5898-192">Tady je kód hello přijímání dat.</span><span class="sxs-lookup"><span data-stu-id="b5898-192">Here is hello code for data ingestion.</span></span>

    # INGEST DATA

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


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="b5898-193">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-193">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-194">Doba trvání tooexecute nad buňku: 51.72 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-194">Time taken tooexecute above cell: 51.72 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="b5898-195">Zkoumání dat a vizualizaci</span><span class="sxs-lookup"><span data-stu-id="b5898-195">Data exploration & visualization</span></span>
<span data-ttu-id="b5898-196">Jakmile hello data vstoupila v Spark, hello dalším krokem v procesu vědecké účely dat hello je toogain podrobnější vysvětlení hello dat prostřednictvím zkoumání a vizualizace.</span><span class="sxs-lookup"><span data-stu-id="b5898-196">Once hello data has been brought into Spark, hello next step in hello data science process is toogain deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="b5898-197">V této části jsme zkontrolujte hello taxíkem dat pomocí dotazů SQL a vykreslení hello cílových proměnných a potenciální funkce pro visual kontroly.</span><span class="sxs-lookup"><span data-stu-id="b5898-197">In this section, we examine hello taxi data using SQL queries and plot hello target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="b5898-198">Konkrétně jsme vykreslení hello frekvenci počty osobní v taxi služebních cest, četnost hello tip objemu a jak se typy liší podle částka platby a typu.</span><span class="sxs-lookup"><span data-stu-id="b5898-198">Specifically, we plot hello frequency of passenger counts in taxi trips, hello frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a><span data-ttu-id="b5898-199">Vykreslení histogram frekvencí počet osobní v ukázce hello taxíkem cest</span><span class="sxs-lookup"><span data-stu-id="b5898-199">Plot a histogram of passenger count frequencies in hello sample of taxi trips</span></span>
<span data-ttu-id="b5898-200">Tento kód a následné fragmenty pomocí SQL magic tooquery hello ukázka a místní magic tooplot hello data.</span><span class="sxs-lookup"><span data-stu-id="b5898-200">This code and subsequent snippets use SQL magic tooquery hello sample and local magic tooplot hello data.</span></span>

* <span data-ttu-id="b5898-201">**SQL magic (`%%sql`)** hello jádra HDInsight PySpark podporuje dotazy na snadno vložené HiveQL pro hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="b5898-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel supports easy inline HiveQL queries against hello sqlContext.</span></span> <span data-ttu-id="b5898-202">Hello (-o VARIABLE_NAME) argument potrvají výstup hello dotazu SQL hello jako Pandas DataFrame na serveru Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-202">hello (-o VARIABLE_NAME) argument persists hello output of hello SQL query as a Pandas DataFrame on hello Jupyter server.</span></span> <span data-ttu-id="b5898-203">To znamená, že je k dispozici v místním režimu hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-203">This means it is available in hello local mode.</span></span>
* <span data-ttu-id="b5898-204">Hello  **`%%local` magic** je použít kód toorun místně na serveru Jupyter hello, což je headnode hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b5898-204">hello **`%%local` magic** is used toorun code locally on hello Jupyter server, which is hello headnode of hello HDInsight cluster.</span></span> <span data-ttu-id="b5898-205">Obvykle použijete, `%%local` magic ve spojení s hello `%%sql` magic s parametrem -o.</span><span class="sxs-lookup"><span data-stu-id="b5898-205">Typically, you use `%%local` magic in conjunction with hello `%%sql` magic with -o parameter.</span></span> <span data-ttu-id="b5898-206">Parametr -o Hello by zachovat hello výstup hello místně v dotazu SQL a pak %% místní magic by aktivovat hello další sadu toorun fragmentu kódu místně proti výstup hello hello dotazů SQL, který je místně trvalé</span><span class="sxs-lookup"><span data-stu-id="b5898-206">hello -o parameter would persist hello output of hello SQL query locally and then %%local magic would trigger hello next set of code snippet toorun locally against hello output of hello SQL queries that is persisted locally</span></span>

<span data-ttu-id="b5898-207">výstup Hello se automaticky vizualizuje po spuštění kódu hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-207">hello output is automatically visualized after you run hello code.</span></span>

<span data-ttu-id="b5898-208">Tento dotaz načte služebních cest hello podle počtu osobní.</span><span class="sxs-lookup"><span data-stu-id="b5898-208">This query retrieves hello trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST hello sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

<span data-ttu-id="b5898-209">Tento kód vytvoří místní data snímku z výstupu dotazu hello a zobrazuje hello data.</span><span class="sxs-lookup"><span data-stu-id="b5898-209">This code creates a local data-frame from hello query output and plots hello data.</span></span> <span data-ttu-id="b5898-210">Hello `%%local` magic vytvoří místní rámce dat, `sqlResults`, který může být použit pro vykreslení s matplotlib.</span><span class="sxs-lookup"><span data-stu-id="b5898-210">hello `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="b5898-211">Tato PySpark magic se používá více než jednou. v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="b5898-211">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="b5898-212">Pokud je velká hello množství dat, by měl ukázkové toocreate data rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="b5898-212">If hello amount of data is large, you should sample toocreate a data-frame that can fit in local memory.</span></span>
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="b5898-213">Zde je osobní hello kód tooplot hello cest počty</span><span class="sxs-lookup"><span data-stu-id="b5898-213">Here is hello code tooplot hello trips by passenger counts</span></span>

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

<span data-ttu-id="b5898-214">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-214">**OUTPUT:**</span></span>

![Frekvence cestě podle počtu osobní](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

<span data-ttu-id="b5898-216">Můžete vybrat mezi několika různých typů vizualizace (tabulky, kruhový, řádku, oblasti nebo panelu) pomocí hello **typ** tlačítka nabídky v poznámkovém bloku hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-216">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using hello **Type** menu buttons in hello notebook.</span></span> <span data-ttu-id="b5898-217">Zobrazí se zde Hello panelu vykreslení.</span><span class="sxs-lookup"><span data-stu-id="b5898-217">hello Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="b5898-218">Vykreslení histogram tip objemy a jak se liší podle počtu a tarif objemy osobní tip velikost.</span><span class="sxs-lookup"><span data-stu-id="b5898-218">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="b5898-219">Použijte data toosample dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="b5898-219">Use a SQL query toosample data.</span></span>

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST hello sqlContext
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


<span data-ttu-id="b5898-220">Tuto buňku kód používá hello SQL dotaz toocreate tři pozemků hello data.</span><span class="sxs-lookup"><span data-stu-id="b5898-220">This code cell uses hello SQL query toocreate three plots hello data.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
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


<span data-ttu-id="b5898-221">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-221">**OUTPUT:**</span></span> 

![Tip rozdělení částky](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip velikost podle velikosti tarif](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="b5898-225">Funkce analýzy, transformaci a data přípravy pro modelování</span><span class="sxs-lookup"><span data-stu-id="b5898-225">Feature engineering, transformation and data preparation for modeling</span></span>
<span data-ttu-id="b5898-226">Tato část popisuje a zajišťuje, že kód hello postupy použít tooprepare dat pro použití v ML modelování.</span><span class="sxs-lookup"><span data-stu-id="b5898-226">This section describes and provides hello code for procedures used tooprepare data for use in ML modeling.</span></span> <span data-ttu-id="b5898-227">Ukazuje, jak toodo hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="b5898-227">It shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="b5898-228">Vytvořit novou funkci přihrádkování čas do provozu čas intervalů</span><span class="sxs-lookup"><span data-stu-id="b5898-228">Create a new feature by binning hours into traffic time buckets</span></span>
* <span data-ttu-id="b5898-229">Index a kódování kategorií funkce</span><span class="sxs-lookup"><span data-stu-id="b5898-229">Index and encode categorical features</span></span>
* <span data-ttu-id="b5898-230">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="b5898-230">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="b5898-231">Vytvořit náhodné vzorky dílčí hello dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="b5898-231">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
* <span data-ttu-id="b5898-232">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="b5898-232">Feature scaling</span></span>
* <span data-ttu-id="b5898-233">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="b5898-233">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="b5898-234">Vytvořit novou funkci přihrádkování čas do provozu čas intervalů</span><span class="sxs-lookup"><span data-stu-id="b5898-234">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="b5898-235">Tento kód ukazuje, jak toocreate novou funkci ve přihrádkování čas do provozu čas intervalů a poté jak toocache hello výsledné datové rámce v paměti.</span><span class="sxs-lookup"><span data-stu-id="b5898-235">This code shows how toocreate a new feature by binning hours into traffic time buckets and then how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="b5898-236">Pokud se opakovaně používají odolné distribuované datové sady (RDDs) a datových rámců, ukládání do mezipaměti vede tooimproved časy spuštění.</span><span class="sxs-lookup"><span data-stu-id="b5898-236">Where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly, caching leads tooimproved execution times.</span></span> <span data-ttu-id="b5898-237">Podle toho jsme mezipaměti RDDs a datové rámce v několika fázích v návodu hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-237">Accordingly, we cache RDDs and data-frames at several stages in hello walkthrough.</span></span> 

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

<span data-ttu-id="b5898-238">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-238">**OUTPUT:**</span></span> 

<span data-ttu-id="b5898-239">126050</span><span class="sxs-lookup"><span data-stu-id="b5898-239">126050</span></span>

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a><span data-ttu-id="b5898-240">Index a kódování kategorií funkce pro vstup do funkce modelování</span><span class="sxs-lookup"><span data-stu-id="b5898-240">Index and encode categorical features for input into modeling functions</span></span>
<span data-ttu-id="b5898-241">Tato část uvádí, jak tooindex nebo kódování kategorií funkce pro vstup do funkce modelování hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-241">This section shows how tooindex or encode categorical features for input into hello modeling functions.</span></span> <span data-ttu-id="b5898-242">Hello modelování a předvídání, funkce MLlib potřeba funkce s kategorií vstupní data toobe indexované nebo kódovaný předchozí toouse.</span><span class="sxs-lookup"><span data-stu-id="b5898-242">hello modeling and predict functions of MLlib require features with categorical input data toobe indexed or encoded prior toouse.</span></span> <span data-ttu-id="b5898-243">V závislosti na modelu hello potřebovat tooindex nebo zakódovat je různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="b5898-243">Depending on hello model, you need tooindex or encode them in different ways:</span></span>  

* <span data-ttu-id="b5898-244">**Na základě stromu modelování** vyžaduje kategorie toobe kódovaná jako číselné hodnoty (například funkce s tří kategorií může být kódován 0, 1, 2).</span><span class="sxs-lookup"><span data-stu-id="b5898-244">**Tree-based modeling** requires categories toobe encoded as numerical values (for example, a feature with three categories may be encoded with 0, 1, 2).</span></span> <span data-ttu-id="b5898-245">To zajišťuje na MLlib [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) funkce.</span><span class="sxs-lookup"><span data-stu-id="b5898-245">This is provided by MLlib’s [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) function.</span></span> <span data-ttu-id="b5898-246">Tato funkce kóduje sloupec řetězce popisky tooa sloupce popisek indexů, které jsou seřazené podle četnosti popisek.</span><span class="sxs-lookup"><span data-stu-id="b5898-246">This function encodes a string column of labels tooa column of label indices that are ordered by label frequencies.</span></span> <span data-ttu-id="b5898-247">I když indexované číselné hodnoty vstupu a manipulaci s daty, algoritmů na základě stromu hello může být zadaný tootreat je správně jako kategorie.</span><span class="sxs-lookup"><span data-stu-id="b5898-247">Although indexed with numerical values for input and data handling, hello tree-based algorithms can be specified tootreat them appropriately as categories.</span></span> 
* <span data-ttu-id="b5898-248">**Modely Logistic a lineární regrese** vyžadují jeden horkou kódování, kde, například funkce s tří kategorií lze rozšířit na tři sloupce funkce, s každou obsahující 0 nebo 1 v závislosti na kategorii hello pozorování.</span><span class="sxs-lookup"><span data-stu-id="b5898-248">**Logistic and Linear Regression models** require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="b5898-249">Poskytuje MLlib [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce toodo horkou jeden kódování.</span><span class="sxs-lookup"><span data-stu-id="b5898-249">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function toodo one-hot encoding.</span></span> <span data-ttu-id="b5898-250">Tato kodér mapuje sloupec popisek indexy tooa sloupce binární vektorů s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b5898-250">This encoder maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="b5898-251">Toto kódování umožňuje algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression funkce toocategorical toobe použít.</span><span class="sxs-lookup"><span data-stu-id="b5898-251">This encoding allows algorithms that expect numerical valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

<span data-ttu-id="b5898-252">Tady je hello tooindex kódu a kódování kategorií funkce:</span><span class="sxs-lookup"><span data-stu-id="b5898-252">Here is hello code tooindex and encode categorical features:</span></span>

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

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

<span data-ttu-id="b5898-253">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-253">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-254">Doba trvání tooexecute nad buňku: 1,28 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-254">Time taken tooexecute above cell: 1.28 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="b5898-255">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="b5898-255">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="b5898-256">Tato část obsahuje kód, který popisuje, jak tooindex kategorií textová data jako datový bod s popiskem typ a zakódovat je tak, aby bylo možné ho použít tootrain a testování MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b5898-256">This section contains code that shows how tooindex categorical text data as a labeled point data type and encode it so that it can be used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="b5898-257">S popiskem bodu objekty jsou odolné distribuované datové sady (RDD) formátu způsobem, který je nutný jako vstupní data pro většinu ML algoritmy v MLlib.</span><span class="sxs-lookup"><span data-stu-id="b5898-257">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="b5898-258">A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.</span><span class="sxs-lookup"><span data-stu-id="b5898-258">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>  

<span data-ttu-id="b5898-259">Tato část obsahuje kód, který ukazuje, jak tooindex kategorií textová data jako [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) datový typ a zakódovat je tak, aby bylo možné ho použít tootrain a testování MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="b5898-259">This section contains code that shows how tooindex categorical text data as a [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) data type and encode it so that it can be used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="b5898-260">S popiskem bodu objekty jsou odolné distribuované datové sady (RDD) skládající se z funkce vector a popisku (cíl a odpověď proměnné).</span><span class="sxs-lookup"><span data-stu-id="b5898-260">Labeled point objects are Resilient Distributed Datasets (RDD) consisting of a label (target/response variable) and feature vector.</span></span> <span data-ttu-id="b5898-261">Tento formát je potřeba jako vstup mnoho algoritmy ML v MLlib.</span><span class="sxs-lookup"><span data-stu-id="b5898-261">This format is needed as input by many ML algorithms in MLlib.</span></span>

<span data-ttu-id="b5898-262">Tady je hello tooindex kódu a kódování textu funkce pro binární klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="b5898-262">Here is hello code tooindex and encode text features for binary classification.</span></span>

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


<span data-ttu-id="b5898-263">Tady je kód hello tooencode a index kategorií text funkce pro analýzu lineární regrese.</span><span class="sxs-lookup"><span data-stu-id="b5898-263">Here is hello code tooencode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="b5898-264">Vytvořit náhodné vzorky dílčí hello dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="b5898-264">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
<span data-ttu-id="b5898-265">Tento kód vytvoří náhodné vzorky dat hello (25 % tady slouží).</span><span class="sxs-lookup"><span data-stu-id="b5898-265">This code creates a random sampling of hello data (25% is used here).</span></span> <span data-ttu-id="b5898-266">I když to není nutné v tomto příkladu kvůli toohello velikost hello datovou sadu, ukážeme, jak můžete vzorkovat tady, víte, jak toouse pro váš vlastní problém v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b5898-266">Although it is not required for this example due toohello size of hello dataset, we demonstrate how you can sample here so you know how toouse it for your own problem when needed.</span></span> <span data-ttu-id="b5898-267">Po velká vzorky lze ušetřit čas významné při školení modelů.</span><span class="sxs-lookup"><span data-stu-id="b5898-267">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="b5898-268">Další jsme rozdělit hello ukázka na školení část (v tomto poli 75 %) a testování toouse část (v tomto poli 25 %) v klasifikaci a regresní modelování.</span><span class="sxs-lookup"><span data-stu-id="b5898-268">Next we split hello sample into a training part (75% here) and a testing part (25% here) toouse in classification and regression modeling.</span></span>

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
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b5898-269">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-269">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-270">Doba trvání tooexecute nad buňku: 0,24 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-270">Time taken tooexecute above cell: 0.24 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="b5898-271">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="b5898-271">Feature scaling</span></span>
<span data-ttu-id="b5898-272">Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-272">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> <span data-ttu-id="b5898-273">Hello kódu pro funkce škálování používá hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello funkce toounit odchylky.</span><span class="sxs-lookup"><span data-stu-id="b5898-273">hello code for feature scaling uses hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello features toounit variance.</span></span> <span data-ttu-id="b5898-274">Pochází od MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD), oblíbených algoritmus pro trénování širokou škálu jiných modelů strojového učení například Vyřešeno regresí nebo support vector počítače (SVM).</span><span class="sxs-lookup"><span data-stu-id="b5898-274">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>

> [!NOTE]
> <span data-ttu-id="b5898-275">Našli jsme hello LinearRegressionWithSGD algoritmus toobe citlivé toofeature škálování.</span><span class="sxs-lookup"><span data-stu-id="b5898-275">We have found hello LinearRegressionWithSGD algorithm toobe sensitive toofeature scaling.</span></span>
> 
> 

<span data-ttu-id="b5898-276">Zde je hello kód tooscale proměnné pro použití s hello Vyřešeno lineární SGD algoritmus.</span><span class="sxs-lookup"><span data-stu-id="b5898-276">Here is hello code tooscale variables for use with hello regularized linear SGD algorithm.</span></span>

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
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b5898-277">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-277">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-278">Doba trvání tooexecute nad buňku: 13.17 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-278">Time taken tooexecute above cell: 13.17 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="b5898-279">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="b5898-279">Cache objects in memory</span></span>
<span data-ttu-id="b5898-280">Hello čas potřebný pro trénování a testování algoritmů ML může snížit ukládání do mezipaměti hello vstupní data rámce objekty používané pro klasifikaci, regrese, a škálovat funkce.</span><span class="sxs-lookup"><span data-stu-id="b5898-280">hello time taken for training and testing of ML algorithms can be reduced by caching hello input data frame objects used for classification, regression, and scaled features.</span></span>

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

<span data-ttu-id="b5898-281">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-281">**OUTPUT:**</span></span> 

<span data-ttu-id="b5898-282">Doba trvání tooexecute nad buňku: 0,15 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-282">Time taken tooexecute above cell: 0.15 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="b5898-283">Předpovědět, zda je tip placené s modely binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="b5898-283">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="b5898-284">Tato část uvádí, jak použití, tři modely pro predikci hello binární klasifikace úlohy zda tip je placené taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="b5898-284">This section shows how use three models for hello binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="b5898-285">modely Hello uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="b5898-285">hello models presented are:</span></span>

* <span data-ttu-id="b5898-286">Vyřešeno logistic regression</span><span class="sxs-lookup"><span data-stu-id="b5898-286">Regularized logistic regression</span></span> 
* <span data-ttu-id="b5898-287">Model náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b5898-287">Random forest model</span></span>
* <span data-ttu-id="b5898-288">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="b5898-288">Gradient Boosting Trees</span></span>

<span data-ttu-id="b5898-289">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="b5898-289">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="b5898-290">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="b5898-290">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="b5898-291">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="b5898-291">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="b5898-292">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="b5898-292">**Saving model** in blob for future consumption</span></span>

### <a name="classification-using-logistic-regression"></a><span data-ttu-id="b5898-293">Klasifikace pomocí logistic regression</span><span class="sxs-lookup"><span data-stu-id="b5898-293">Classification using logistic regression</span></span>
<span data-ttu-id="b5898-294">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b5898-294">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

<span data-ttu-id="b5898-295">**Cvičení hello logistic regresní model pomocí odchylka nákladů a hyperparameter (vymetání) komínů**</span><span class="sxs-lookup"><span data-stu-id="b5898-295">**Train hello logistic regression model using CV and hyperparameter sweeping**</span></span>

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

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="b5898-296">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-296">**OUTPUT:**</span></span> 

<span data-ttu-id="b5898-297">Koeficienty: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="b5898-297">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="b5898-298">Zachycení:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="b5898-298">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="b5898-299">Doba trvání tooexecute nad buňku: 14.43 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-299">Time taken tooexecute above cell: 14.43 seconds</span></span>

<span data-ttu-id="b5898-300">**Vyhodnocení modelu binární klasifikace hello o standardní metriky**</span><span class="sxs-lookup"><span data-stu-id="b5898-300">**Evaluate hello binary classification model with standard metrics**</span></span>

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
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="b5898-301">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-301">**OUTPUT:**</span></span> 

<span data-ttu-id="b5898-302">Oblasti v rámci PR = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="b5898-302">Area under PR = 0.985297691373</span></span>

<span data-ttu-id="b5898-303">Oblasti v rámci ROC = 0.983714670256</span><span class="sxs-lookup"><span data-stu-id="b5898-303">Area under ROC = 0.983714670256</span></span>

<span data-ttu-id="b5898-304">Souhrnné statistiky</span><span class="sxs-lookup"><span data-stu-id="b5898-304">Summary Stats</span></span>

<span data-ttu-id="b5898-305">Přesnost = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="b5898-305">Precision = 0.984304060189</span></span>

<span data-ttu-id="b5898-306">Odvolat = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="b5898-306">Recall = 0.984304060189</span></span>

<span data-ttu-id="b5898-307">F1 Stanovení skóre = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="b5898-307">F1 Score = 0.984304060189</span></span>

<span data-ttu-id="b5898-308">Doba trvání tooexecute nad buňku: 57.61 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-308">Time taken tooexecute above cell: 57.61 seconds</span></span>

<span data-ttu-id="b5898-309">**Vykreslení křivka ROC hello.**</span><span class="sxs-lookup"><span data-stu-id="b5898-309">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="b5898-310">Hello *predictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-310">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="b5898-311">*tmp_results* je možné použít toodo dotazy a výstup výsledků do hello sqlResults dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="b5898-311">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="b5898-312">Tady je kód hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-312">Here is hello code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="b5898-313">Zde je hello kód toomake předpovědi a vykreslení hello křivka ROC.</span><span class="sxs-lookup"><span data-stu-id="b5898-313">Here is hello code toomake predictions and plot hello ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
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


<span data-ttu-id="b5898-314">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-314">**OUTPUT:**</span></span>

![Logistic regression ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="b5898-316">Klasifikace náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b5898-316">Random forest classification</span></span>
<span data-ttu-id="b5898-317">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit model náhodných doménové struktury, který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b5898-317">hello code in this section shows how tootrain, evaluate, and save a random forest model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

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
    ## UN-COMMENT IF YOU WANT tooPRINT TREES
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

<span data-ttu-id="b5898-318">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-318">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-319">Oblasti v rámci ROC = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="b5898-319">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="b5898-320">Doba trvání tooexecute nad buňku: 31.09 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-320">Time taken tooexecute above cell: 31.09 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="b5898-321">Přechodu zvýšení skóre klasifikace stromů</span><span class="sxs-lookup"><span data-stu-id="b5898-321">Gradient boosting trees classification</span></span>
<span data-ttu-id="b5898-322">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b5898-322">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
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
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="b5898-323">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-323">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-324">Oblasti v rámci ROC = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="b5898-324">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="b5898-325">Doba trvání tooexecute nad buňku: 19.76 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-325">Time taken tooexecute above cell: 19.76 seconds</span></span>

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a><span data-ttu-id="b5898-326">Předpověď objemy tip pro služebních cest taxíkem s modely regrese</span><span class="sxs-lookup"><span data-stu-id="b5898-326">Predict tip amounts for taxi trips with regression models</span></span>
<span data-ttu-id="b5898-327">Tato část uvádí, jak použít, tři modely pro predikci hello množství hello tip placené taxíkem cesty na základě jiných funkcí tip hello regrese úlohy.</span><span class="sxs-lookup"><span data-stu-id="b5898-327">This section shows how use three models for hello regression task of predicting hello amount of hello tip paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="b5898-328">modely Hello uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="b5898-328">hello models presented are:</span></span>

* <span data-ttu-id="b5898-329">Vyřešeno lineární regrese</span><span class="sxs-lookup"><span data-stu-id="b5898-329">Regularized linear regression</span></span>
* <span data-ttu-id="b5898-330">Náhodné doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b5898-330">Random forest</span></span>
* <span data-ttu-id="b5898-331">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="b5898-331">Gradient Boosting Trees</span></span>

<span data-ttu-id="b5898-332">Tyto modely byly popsané v úvodu hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-332">These models were described in hello introduction.</span></span> <span data-ttu-id="b5898-333">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="b5898-333">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="b5898-334">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="b5898-334">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="b5898-335">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="b5898-335">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="b5898-336">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="b5898-336">**Saving model** in blob for future consumption</span></span>

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="b5898-337">Lineární regrese s SGD</span><span class="sxs-lookup"><span data-stu-id="b5898-337">Linear regression with SGD</span></span>
<span data-ttu-id="b5898-338">Hello kód v této části ukazuje, jak toouse škálovat funkce tootrain lineární regrese, který stochastického přechodu klesání (SGD) používá pro optimalizaci, a jak tooscore, hodnocení a uložit hello model v Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="b5898-338">hello code in this section shows how toouse scaled features tootrain a linear regression that uses stochastic gradient descent (SGD) for optimization, and how tooscore, evaluate, and save hello model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="b5898-339">V našich zkušeností může být problémy s hello konvergence LinearRegressionWithSGD modelů a parametry potřebovat toobe změnit/optimalizované pečlivě pro získání platný model.</span><span class="sxs-lookup"><span data-stu-id="b5898-339">In our experience, there can be issues with hello convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="b5898-340">Škálování proměnných výrazně pomáhá s konvergence.</span><span class="sxs-lookup"><span data-stu-id="b5898-340">Scaling of variables significantly helps with convergence.</span></span> 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

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

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN hello DEFAULT BLOB FOR hello CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b5898-341">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-341">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-342">Koeficienty: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]</span><span class="sxs-lookup"><span data-stu-id="b5898-342">Coefficients: [0.00457675809917, -0.0226314167349, -0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981, -0.000987181489428, -0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995, -0.00990211159703, -0.00637410344522, 0.545083566179, -0.536756072402, 0.0105762393099, -0.0130117577055, 0.0129304737772, -0.00171065945959]</span></span>

<span data-ttu-id="b5898-343">Zachytávat: 0.853872718283</span><span class="sxs-lookup"><span data-stu-id="b5898-343">Intercept: 0.853872718283</span></span>

<span data-ttu-id="b5898-344">RMSE = 1.24190115863</span><span class="sxs-lookup"><span data-stu-id="b5898-344">RMSE = 1.24190115863</span></span>

<span data-ttu-id="b5898-345">R sqr = 0.608017146081</span><span class="sxs-lookup"><span data-stu-id="b5898-345">R-sqr = 0.608017146081</span></span>

<span data-ttu-id="b5898-346">Doba trvání tooexecute nad buňku: 58.42 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-346">Time taken tooexecute above cell: 58.42 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="b5898-347">Regrese náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="b5898-347">Random Forest regression</span></span>
<span data-ttu-id="b5898-348">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit náhodných doménové struktury regrese, který bude předpovídat velikost tip pro hello NYC taxíkem cestě data.</span><span class="sxs-lookup"><span data-stu-id="b5898-348">hello code in this section shows how tootrain, evaluate, and save a random forest regression that predicts tip amount for hello NYC taxi trip data.</span></span>

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
    ## UN-COMMENT IF YOU WANT tooPRING TREES
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
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b5898-349">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-349">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-350">RMSE = 0.891209218139</span><span class="sxs-lookup"><span data-stu-id="b5898-350">RMSE = 0.891209218139</span></span>

<span data-ttu-id="b5898-351">R sqr = 0.759661334921</span><span class="sxs-lookup"><span data-stu-id="b5898-351">R-sqr = 0.759661334921</span></span>

<span data-ttu-id="b5898-352">Doba trvání tooexecute nad buňku: 49.21 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-352">Time taken tooexecute above cell: 49.21 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="b5898-353">Přechodu zvýšení skóre regresní stromy</span><span class="sxs-lookup"><span data-stu-id="b5898-353">Gradient boosting trees regression</span></span>
<span data-ttu-id="b5898-354">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá velikost tip pro hello NYC taxíkem cestě data.</span><span class="sxs-lookup"><span data-stu-id="b5898-354">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts tip amount for hello NYC taxi trip data.</span></span>

<span data-ttu-id="b5898-355">** Natrénování a vyhodnocení **</span><span class="sxs-lookup"><span data-stu-id="b5898-355">**Train and evaluate **</span></span>

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

    # CONVER RESULTS tooDF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="b5898-356">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-356">**OUTPUT:**</span></span>

<span data-ttu-id="b5898-357">RMSE = 0.908473148639</span><span class="sxs-lookup"><span data-stu-id="b5898-357">RMSE = 0.908473148639</span></span>

<span data-ttu-id="b5898-358">R sqr = 0.753835096681</span><span class="sxs-lookup"><span data-stu-id="b5898-358">R-sqr = 0.753835096681</span></span>

<span data-ttu-id="b5898-359">Doba trvání tooexecute nad buňku: 34.52 sekund</span><span class="sxs-lookup"><span data-stu-id="b5898-359">Time taken tooexecute above cell: 34.52 seconds</span></span>

<span data-ttu-id="b5898-360">**Vykreslení.**</span><span class="sxs-lookup"><span data-stu-id="b5898-360">**Plot**</span></span>

<span data-ttu-id="b5898-361">*tmp_results* je registrován jako tabulku Hive v předchozí buňku hello.</span><span class="sxs-lookup"><span data-stu-id="b5898-361">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="b5898-362">Výsledky z tabulky hello jsou výstupem do hello *sqlResults* dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="b5898-362">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="b5898-363">Tady je kód hello</span><span class="sxs-lookup"><span data-stu-id="b5898-363">Here is hello code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

<span data-ttu-id="b5898-364">Zde je hello kód tooplot hello dat pomocí hello Jupyter server.</span><span class="sxs-lookup"><span data-stu-id="b5898-364">Here is hello code tooplot hello data using hello Jupyter server.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
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


<span data-ttu-id="b5898-365">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="b5898-365">**OUTPUT:**</span></span>

![Skutečný vs předpovědět tip objemy](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a><span data-ttu-id="b5898-367">Vyčištění objektů z paměti</span><span class="sxs-lookup"><span data-stu-id="b5898-367">Clean up objects from memory</span></span>
<span data-ttu-id="b5898-368">Použití `unpersist()` toodelete objekty uložené v mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="b5898-368">Use `unpersist()` toodelete objects cached in memory.</span></span>

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


## <a name="record-storage-locations-of-hello-models-for-consumption-and-scoring"></a><span data-ttu-id="b5898-369">Umístění úložiště záznam hello modelů pro využívání a vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="b5898-369">Record storage locations of hello models for consumption and scoring</span></span>
<span data-ttu-id="b5898-370">tooconsume a skóre nezávislé datové sady popsané hello [skóre a vyhodnocení modelů vytvořené Spark strojové učení](machine-learning-data-science-spark-model-consumption.md) tématu, musíte toocopy a vložit názvy těchto souborů obsahující hello uložit modely vytvořená zde do hello Spotřeba Poznámkový blok Jupyter.</span><span class="sxs-lookup"><span data-stu-id="b5898-370">tooconsume and score an independent dataset described in hello [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) topic, you need toocopy and paste these file names containing hello saved models created here into hello Consumption Jupyter notebook.</span></span> <span data-ttu-id="b5898-371">Zde je kód tooprint hello se hello cesty toomodel soubory, které potřebujete existuje.</span><span class="sxs-lookup"><span data-stu-id="b5898-371">Here is hello code tooprint out hello paths toomodel files you need there.</span></span>

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="b5898-372">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="b5898-372">**OUTPUT**</span></span>

<span data-ttu-id="b5898-373">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"</span><span class="sxs-lookup"><span data-stu-id="b5898-373">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"</span></span>

<span data-ttu-id="b5898-374">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"</span><span class="sxs-lookup"><span data-stu-id="b5898-374">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"</span></span>

<span data-ttu-id="b5898-375">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"</span><span class="sxs-lookup"><span data-stu-id="b5898-375">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"</span></span>

<span data-ttu-id="b5898-376">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"</span><span class="sxs-lookup"><span data-stu-id="b5898-376">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"</span></span>

<span data-ttu-id="b5898-377">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"</span><span class="sxs-lookup"><span data-stu-id="b5898-377">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"</span></span>

<span data-ttu-id="b5898-378">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"</span><span class="sxs-lookup"><span data-stu-id="b5898-378">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"</span></span>

## <a name="whats-next"></a><span data-ttu-id="b5898-379">Co dále?</span><span class="sxs-lookup"><span data-stu-id="b5898-379">What's next?</span></span>
<span data-ttu-id="b5898-380">Teď, když jste vytvořili regrese a klasifikace modely s hello Spark MlLib, jak jste připravené toolearn tooscore a vyhodnocovat u nich těchto modelů.</span><span class="sxs-lookup"><span data-stu-id="b5898-380">Now that you have created regression and classification models with hello Spark MlLib, you are ready toolearn how tooscore and evaluate these models.</span></span> <span data-ttu-id="b5898-381">Hello advanced zkoumání dat a modelování poznámkového bloku dives hlubší do včetně křížového ověřování, technologie hyper parametr komínů a vyhodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="b5898-381">hello advanced data exploration and modeling notebook dives deeper into including cross-validation, hyper-parameter sweeping, and model evaluation.</span></span> 

<span data-ttu-id="b5898-382">**Model spotřeby:** toolearn jak tooscore a vyhodnotit hello klasifikace a regrese modelů vytvořených v tomto tématu najdete v tématu [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="b5898-382">**Model consumption:** toolearn how tooscore and evaluate hello classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

<span data-ttu-id="b5898-383">**Křížové ověření a hyperparameter (vymetání) komínů**: najdete v části [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí (vymetání) křížové ověření a technologie hyper parametr komínů</span><span class="sxs-lookup"><span data-stu-id="b5898-383">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping</span></span>

