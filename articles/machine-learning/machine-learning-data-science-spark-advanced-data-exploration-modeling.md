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
# <a name="advanced-data-exploration-and-modeling-with-spark"></a><span data-ttu-id="25376-103">Pokročilé zkoumání a modelování dat pomocí Spark</span><span class="sxs-lookup"><span data-stu-id="25376-103">Advanced data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="25376-104">Tento návod používá HDInsight Spark toodo zkoumání a train binární klasifikace dat a modely regrese pomocí křížové ověření a hyperparameter optimalizace na základě vzorku hello NYC taxi cestě a jízdenky 2013 datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="25376-104">This walkthrough uses HDInsight Spark toodo data exploration and train binary classification and regression models using cross-validation and hyperparameter optimization on a sample of hello NYC taxi trip and fare 2013 dataset.</span></span> <span data-ttu-id="25376-105">Ho vás provede kroky hello hello [proces vědecké účely dat](http://aka.ms/datascienceprocess), klient server, pomocí HDInsight Spark clusteru pro zpracování a toostore hello dat a hello modely objektů Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="25376-105">It walks you through hello steps of hello [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs toostore hello data and hello models.</span></span> <span data-ttu-id="25376-106">proces Hello jsou zde popsány vizualizuje dat získaných z objektu Blob úložiště Azure a pak připraví hello data toobuild prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="25376-106">hello process explores and visualizes data brought in from an Azure Storage Blob and then prepares hello data toobuild predictive models.</span></span> <span data-ttu-id="25376-107">Python byl použité toocode hello řešení a tooshow hello relevantní pozemků.</span><span class="sxs-lookup"><span data-stu-id="25376-107">Python has been used toocode hello solution and tooshow hello relevant plots.</span></span> <span data-ttu-id="25376-108">Tyto modely jsou sestavení pomocí hello Spark MLlib toolkit toodo binární klasifikace a regresní modelování úlohy.</span><span class="sxs-lookup"><span data-stu-id="25376-108">These models are build using hello Spark MLlib toolkit toodo binary classification and regression modeling tasks.</span></span> 

* <span data-ttu-id="25376-109">Hello **binární klasifikace** úloha je toopredict, zda je pro cestu hello placené tip.</span><span class="sxs-lookup"><span data-stu-id="25376-109">hello **binary classification** task is toopredict whether or not a tip is paid for hello trip.</span></span> 
* <span data-ttu-id="25376-110">Hello **regrese** úloh je toopredict hello množství hello tip podle dalších funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="25376-110">hello **regression** task is toopredict hello amount of hello tip based on other tip features.</span></span> 

<span data-ttu-id="25376-111">Hello modelování kroky také obsahovat kódu, který ukazuje, jak tootrain, hodnocení a uložit každý typ modelu.</span><span class="sxs-lookup"><span data-stu-id="25376-111">hello modeling steps also contain code showing how tootrain, evaluate, and save each type of model.</span></span> <span data-ttu-id="25376-112">Hello téma popisuje některé z hello stejné pozadí jako hello [zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="25376-112">hello topic covers some of hello same ground as hello [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic.</span></span> <span data-ttu-id="25376-113">Ale ho je "složitější," také používá křížového ověřování s hyperparameter komínů tootrain optimálně přesné klasifikace a regrese modelů.</span><span class="sxs-lookup"><span data-stu-id="25376-113">But it is more "advanced" in that it also uses cross-validation with hyperparameter sweeping tootrain optimally accurate classification and regression models.</span></span> 

<span data-ttu-id="25376-114">**Křížové ověření (odchylka nákladů)** je technika, který vyhodnocuje, jak dobře model trénink na známé sadu dat umožňuje zobecnit toopredicting hello funkce datových sad, na kterém je nebyla cvičena.</span><span class="sxs-lookup"><span data-stu-id="25376-114">**Cross-validation (CV)** is a technique that assesses how well a model trained on a known set of data generalizes toopredicting hello features of datasets on which it has not been trained.</span></span>  <span data-ttu-id="25376-115">Běžná implementace použít zde je toodivide datové sady do tisíc složení a pak trénování modelu hello v kruhového dotazování na všechny kromě jednoho hello složení.</span><span class="sxs-lookup"><span data-stu-id="25376-115">A common implementation used here is toodivide a dataset into K folds and then train hello model in a round-robin fashion on all but one of hello folds.</span></span> <span data-ttu-id="25376-116">se hodnotí schopnost Hello hello modelu tooprediction přesně, při testování proti hello nezávislé datové sady v tomto složení nepoužívá tootrain hello modelu.</span><span class="sxs-lookup"><span data-stu-id="25376-116">hello ability of hello model tooprediction accurately when tested against hello independent dataset in this fold not used tootrain hello model is assessed.</span></span>

<span data-ttu-id="25376-117">**Optimalizace Hyperparameter** potížím hello vybrat sadu hyperparameters pro algoritmu učení, obvykle s cílem hello optimalizace měření výkonu hello algoritmus na nezávislé datové sady.</span><span class="sxs-lookup"><span data-stu-id="25376-117">**Hyperparameter optimization** is hello problem of choosing a set of hyperparameters for a learning algorithm, usually with hello goal of optimizing a measure of hello algorithm's performance on an independent data set.</span></span> <span data-ttu-id="25376-118">**Hyperparameters** jsou hodnoty, které musí být zadán mimo hello modelu školení postupu.</span><span class="sxs-lookup"><span data-stu-id="25376-118">**Hyperparameters** are values that must be specified outside of hello model training procedure.</span></span> <span data-ttu-id="25376-119">Předpoklady o tyto hodnoty může ovlivnit hello flexibilitu a přesnost hello modelů.</span><span class="sxs-lookup"><span data-stu-id="25376-119">Assumptions about these values can impact hello flexibility and accuracy of hello models.</span></span> <span data-ttu-id="25376-120">Rozhodovací stromy mít hyperparameters, například jako hello potřeby hloubkou a počet nechá ve stromu hello.</span><span class="sxs-lookup"><span data-stu-id="25376-120">Decision trees have hyperparameters, for example, such as hello desired depth and number of leaves in hello tree.</span></span> <span data-ttu-id="25376-121">Podpora vektoru počítače (SVMs) vyžadují, nastavení termín snížení chybnou klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="25376-121">Support Vector Machines (SVMs) require setting a misclassification penalty term.</span></span> 

<span data-ttu-id="25376-122">Běžné způsob tooperform hyperparameter optimalizace použít zde je hledání mřížky nebo **oblouku parametr**.</span><span class="sxs-lookup"><span data-stu-id="25376-122">A common way tooperform hyperparameter optimization used here is a grid search, or a **parameter sweep**.</span></span> <span data-ttu-id="25376-123">Tento postup se skládá provádět důkladné prohledání prostřednictvím hello hodnoty podmnožinu hello hyperparameter místo zadané pro algoritmus učení.</span><span class="sxs-lookup"><span data-stu-id="25376-123">This consists of performing an exhaustive search through hello values a specified subset of hello hyperparameter space for a learning algorithm.</span></span> <span data-ttu-id="25376-124">Křížového ověření může poskytovat metriky toosort out hello optimální výsledky vyprodukované hello mřížky vyhledávacího algoritmu výkonu.</span><span class="sxs-lookup"><span data-stu-id="25376-124">Cross validation can supply a performance metric toosort out hello optimal results produced by hello grid search algorithm.</span></span> <span data-ttu-id="25376-125">Odchylka nákladů použít s obezřetností pomáhá hyperparameter limit problémy jako overfitting tootraining datový model, aby hello modelu uchovává hello kapacity tooapply toohello obecné sady dat, ze které hello jste extrahovali Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="25376-125">CV used with hyperparameter sweeping helps limit problems like overfitting a model tootraining data so that hello model retains hello capacity tooapply toohello general set of data from which hello training data was extracted.</span></span>

<span data-ttu-id="25376-126">Hello modely, které používáme zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromů:</span><span class="sxs-lookup"><span data-stu-id="25376-126">hello models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="25376-127">[Lineární regrese s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je model lineární regrese, který používá metodu Stochastického přechodu klesání (SGD) a škálování toopredict hello tip objemy placené pro optimalizaci a funkce.</span><span class="sxs-lookup"><span data-stu-id="25376-127">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling toopredict hello tip amounts paid.</span></span> 
* <span data-ttu-id="25376-128">[Logistic regression s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) nebo "logit" regression je regresní model, který lze použít při hello závislé proměnné je klasifikace dat kategorií toodo.</span><span class="sxs-lookup"><span data-stu-id="25376-128">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when hello dependent variable is categorical toodo data classification.</span></span> <span data-ttu-id="25376-129">LBFGS je algoritmus optimalizace jako Newton, blíží algoritmus hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomocí omezené množství paměti počítače a který se často používá v machine learning.</span><span class="sxs-lookup"><span data-stu-id="25376-129">LBFGS is a quasi-Newton optimization algorithm that approximates hello Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="25376-130">[Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="25376-130">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="25376-131">Že kombinují mnoho rozhodovací stromy tooreduce hello riziko overfitting.</span><span class="sxs-lookup"><span data-stu-id="25376-131">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="25376-132">Náhodné doménové struktury se používají pro regresní a klasifikace a dokáže zpracovat kategorií funkce a je možné rozšířit toohello více třídami klasifikace nastavení.</span><span class="sxs-lookup"><span data-stu-id="25376-132">Random forests are used for regression and classification and can handle categorical features and can be extended toohello multiclass classification setting.</span></span> <span data-ttu-id="25376-133">Funkce škálování a jsou možné toocapture nelineárností a funkce interakce se nevyžadují.</span><span class="sxs-lookup"><span data-stu-id="25376-133">They do not require feature scaling and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="25376-134">Náhodné doménových strukturách jsou jedním z hello těch nejúspěšnějších modelů strojového učení pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="25376-134">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="25376-135">[Přechodu boosted stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="25376-135">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="25376-136">GBTs train rozhodovací stromy interaktivně toominimize funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="25376-136">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="25376-137">GBTs se používají pro regresní a klasifikace a dokáže zpracovat kategorií funkce, nevyžadují funkce škálování a mít toocapture nelineárností a funkce, interakce.</span><span class="sxs-lookup"><span data-stu-id="25376-137">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="25376-138">Můžete také používají v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="25376-138">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="25376-139">Modelování příklady použití odchylka nákladů a Hyperparameter oblouku se zobrazují pro problém binární klasifikace hello.</span><span class="sxs-lookup"><span data-stu-id="25376-139">Modeling examples using CV and Hyperparameter sweep are shown for hello binary classification problem.</span></span> <span data-ttu-id="25376-140">Jednodušší příklady (bez parametru změny) jsou uvedeny na hello hlavní téma pro regresní úlohy.</span><span class="sxs-lookup"><span data-stu-id="25376-140">Simpler examples (without parameter sweeps) are presented in hello main topic for regression tasks.</span></span> <span data-ttu-id="25376-141">Ale v hello příloha, jsou také uvedeny ověření pomocí elastické net pro lineární regrese a odchylka nákladů pomocí parametru oblouku pro regresní náhodných doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="25376-141">But in hello appendix, validation using elastic net for linear regression and CV with parameter sweep using for random forest regression are also presented.</span></span> <span data-ttu-id="25376-142">Hello **Elastická net** je metoda Vyřešeno regrese pro hodí lineární regrese modelů, Lineárně kombinuje hello L1 a L2 metriky jako postihy hello [laso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) a [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) metody.</span><span class="sxs-lookup"><span data-stu-id="25376-142">hello **elastic net** is a regularized regression method for fitting linear regression models that linearly combines hello L1 and L2 metrics as penalties of hello [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) and [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) methods.</span></span>   

> [!NOTE]
> <span data-ttu-id="25376-143">Sice hello Spark MLlib toolkit navrženou toowork v rozsáhlých datových sad, relativně malé ukázkové (pomocí 170 tisíc řádků, přibližně 0,1 % hello původní datové sady NYC ~ 30 Mb) se zde používá ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="25376-143">Although hello Spark MLlib toolkit is designed toowork on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of hello original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="25376-144">cvičení Hello zadané tady běží efektivně (v přibližně 10 minut) v clusteru HDInsight s 2 uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="25376-144">hello exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="25376-145">Hello stejný kód s méně závažné změny lze použít tooprocess větší-sady dat, se změny, které pro ukládání do mezipaměti data v paměti a změna velikosti clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="25376-145">hello same code, with minor modifications, can be used tooprocess larger data-sets, with appropriate modifications for caching data in memory and changing hello cluster size.</span></span>
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a><span data-ttu-id="25376-146">Instalační program: Clustery Spark a poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="25376-146">Setup: Spark clusters and notebooks</span></span>
<span data-ttu-id="25376-147">Kroky instalace a kódu jsou uvedené v tomto názorném postupu pro používání HDInsight Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="25376-147">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="25376-148">Ale poznámkové bloky Jupyter jsou k dispozici pro clustery HDInsight Spark 1.6 a Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="25376-148">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="25376-149">Popis toothem hello poznámkových bloků a odkazy jsou uvedeny v hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) hello Githubu úložiště, které je obsahují.</span><span class="sxs-lookup"><span data-stu-id="25376-149">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="25376-150">Kromě toho hello kód sem a v poznámkových bloků hello propojené je obecný a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="25376-150">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="25376-151">Pokud nepoužíváte HDInsight Spark, může být kroky instalace a správy clusteru hello mírně lišit od co se zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="25376-151">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="25376-152">Pro usnadnění práce uvádíme hello odkazy toohello Jupyter notebooks Spark 1.6 a 2.0 toobe spustit v jádra pyspark hello Dobrý den Poznámkový blok Jupyter serveru:</span><span class="sxs-lookup"><span data-stu-id="25376-152">For convenience, here are hello links toohello Jupyter notebooks for Spark 1.6 and 2.0 toobe run in hello pyspark kernel of hello Jupyter Notebook server:</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="25376-153">Spark 1.6 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="25376-153">Spark 1.6 notebooks</span></span>

<span data-ttu-id="25376-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): obsahuje témata v poznámkovém bloku #1 a modelu vývoj pomocí hyperparameter ladění a křížové ověření.</span><span class="sxs-lookup"><span data-stu-id="25376-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Includes topics in notebook #1, and model development using hyperparameter tuning and cross-validation.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="25376-155">Spark 2.0 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="25376-155">Spark 2.0 notebooks</span></span>

<span data-ttu-id="25376-156">[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Tento soubor obsahuje informace o tom, jak tooperform zkoumání dat, modelování a vyhodnocování v rámci Spark 2.0 clusterů.</span><span class="sxs-lookup"><span data-stu-id="25376-156">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how tooperform data exploration, modeling, and scoring in Spark 2.0 clusters.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="25376-157">Instalační program: umístění úložiště, knihovny a hello přednastavení Spark kontextu</span><span class="sxs-lookup"><span data-stu-id="25376-157">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="25376-158">Spark je možné tooAzure tooread a zápisu objektu Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="25376-158">Spark is able tooread and write tooAzure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="25376-159">Takže existující data v ní uloženy může zpracovat pomocí Spark a hello výsledky uložené v znovu WASB.</span><span class="sxs-lookup"><span data-stu-id="25376-159">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="25376-160">modely toosave či soubory v WASB, musí cesta hello toobe zadán správně.</span><span class="sxs-lookup"><span data-stu-id="25376-160">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="25376-161">Hello clusteru Spark toohello výchozí kontejner připojené můžete odkazovat pomocí cesty počínaje: "wasb: / / /".</span><span class="sxs-lookup"><span data-stu-id="25376-161">hello default container attached toohello Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="25376-162">Odkazují jiné umístění "wasb: / /".</span><span class="sxs-lookup"><span data-stu-id="25376-162">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="25376-163">Nastavení cesty adresáře pro umístění úložiště v WASB</span><span class="sxs-lookup"><span data-stu-id="25376-163">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="25376-164">Hello následující ukázka kódu určuje hello umístění toobe hello data přečíst a uložena hello cesta pro model výstup hello directory toowhich hello modelu úložiště:</span><span class="sxs-lookup"><span data-stu-id="25376-164">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved:</span></span>

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="25376-165">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-165">**OUTPUT**</span></span>

<span data-ttu-id="25376-166">DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)</span><span class="sxs-lookup"><span data-stu-id="25376-166">datetime.datetime(2016, 4, 18, 17, 36, 27, 832799)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="25376-167">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="25376-167">Import libraries</span></span>
<span data-ttu-id="25376-168">Importujte knihovny potřebné s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="25376-168">Import necessary libraries with hello following code:</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="25376-169">Předvolby kontextu Spark a Magic PySpark</span><span class="sxs-lookup"><span data-stu-id="25376-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="25376-170">Hello jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu.</span><span class="sxs-lookup"><span data-stu-id="25376-170">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="25376-171">Proto není nutné tooset kontexty Spark nebo Hive hello explicitně před zahájením práce s hello aplikací, které vyvíjíte.</span><span class="sxs-lookup"><span data-stu-id="25376-171">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="25376-172">Tyto kontexty jsou dostupné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="25376-172">These contexts are available for you by default.</span></span> <span data-ttu-id="25376-173">Tyto kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="25376-173">These contexts are:</span></span>

* <span data-ttu-id="25376-174">sc - pro Spark</span><span class="sxs-lookup"><span data-stu-id="25376-174">sc - for Spark</span></span> 
* <span data-ttu-id="25376-175">sqlContext - pro Hive</span><span class="sxs-lookup"><span data-stu-id="25376-175">sqlContext - for Hive</span></span>

<span data-ttu-id="25376-176">Hello jádra PySpark poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%.</span><span class="sxs-lookup"><span data-stu-id="25376-176">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="25376-177">Existují dva takové příkazy, které se používají v tyto ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="25376-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="25376-178">**%% místní** Určuje, že je kód hello v dalších řádcích toobe provést místně.</span><span class="sxs-lookup"><span data-stu-id="25376-178">**%%local** Specifies that hello code in subsequent lines is toobe executed locally.</span></span> <span data-ttu-id="25376-179">Kód musí být platný kód Python.</span><span class="sxs-lookup"><span data-stu-id="25376-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="25376-180">**%% sql -o <variable name>**  provede dotaz Hive proti hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="25376-180">**%%sql -o <variable name>** Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="25376-181">Pokud není předán parametr -o hello hello výsledek dotazu hello je uchován v hello %% lokální kontext Python jako Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="25376-181">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="25376-182">Pro další informace o hello jádra pro poznámkové bloky Jupyter a hello předdefinované "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="25376-182">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="25376-183">Přijímání dat z veřejného objektu blob:</span><span class="sxs-lookup"><span data-stu-id="25376-183">Data ingestion from public blob:</span></span>
<span data-ttu-id="25376-184">Hello prvním krokem v procesu vědecké účely dat hello je tooingest hello data toobe analyzovali ze zdrojů, na němž je umístěna do prostředí pro zkoumání a modelování aplikace data.</span><span class="sxs-lookup"><span data-stu-id="25376-184">hello first step in hello data science process is tooingest hello data toobe analyzed from sources where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="25376-185">Toto prostředí je Spark v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="25376-185">This environment is Spark in this walkthrough.</span></span> <span data-ttu-id="25376-186">Tato část obsahuje hello kód toocomplete řadu úloh:</span><span class="sxs-lookup"><span data-stu-id="25376-186">This section contains hello code toocomplete a series of tasks:</span></span>

* <span data-ttu-id="25376-187">ingestování toobe ukázková data hello modelován</span><span class="sxs-lookup"><span data-stu-id="25376-187">ingest hello data sample toobe modeled</span></span>
* <span data-ttu-id="25376-188">Přečtěte si hello vstupní datové sady (uložený jako soubor TSV)</span><span class="sxs-lookup"><span data-stu-id="25376-188">read in hello input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="25376-189">Formát a vyčištění hello data</span><span class="sxs-lookup"><span data-stu-id="25376-189">format and clean hello data</span></span>
* <span data-ttu-id="25376-190">vytvářet a ukládat do mezipaměti objektů (RDDs nebo datových rámců) v paměti</span><span class="sxs-lookup"><span data-stu-id="25376-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="25376-191">Zaregistrujte se jako dočasné tabulky v kontextu SQL.</span><span class="sxs-lookup"><span data-stu-id="25376-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="25376-192">Tady je kód hello přijímání dat.</span><span class="sxs-lookup"><span data-stu-id="25376-192">Here is hello code for data ingestion.</span></span>

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


<span data-ttu-id="25376-193">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-193">**OUTPUT**</span></span>

<span data-ttu-id="25376-194">Doba trvání tooexecute nad buňku: 276.62 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-194">Time taken tooexecute above cell: 276.62 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="25376-195">Zkoumání dat a vizualizaci</span><span class="sxs-lookup"><span data-stu-id="25376-195">Data exploration & visualization</span></span>
<span data-ttu-id="25376-196">Jakmile hello data vstoupila v Spark, hello dalším krokem v procesu vědecké účely dat hello je toogain podrobnější vysvětlení hello dat prostřednictvím zkoumání a vizualizace.</span><span class="sxs-lookup"><span data-stu-id="25376-196">Once hello data has been brought into Spark, hello next step in hello data science process is toogain deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="25376-197">V této části jsme zkontrolujte hello taxíkem dat pomocí dotazů SQL a vykreslení hello cílových proměnných a potenciální funkce pro visual kontroly.</span><span class="sxs-lookup"><span data-stu-id="25376-197">In this section, we examine hello taxi data using SQL queries and plot hello target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="25376-198">Konkrétně jsme vykreslení hello frekvenci počty osobní v taxi služebních cest, četnost hello tip objemu a jak se typy liší podle částka platby a typu.</span><span class="sxs-lookup"><span data-stu-id="25376-198">Specifically, we plot hello frequency of passenger counts in taxi trips, hello frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a><span data-ttu-id="25376-199">Vykreslení histogram frekvencí počet osobní v ukázce hello taxíkem cest</span><span class="sxs-lookup"><span data-stu-id="25376-199">Plot a histogram of passenger count frequencies in hello sample of taxi trips</span></span>
<span data-ttu-id="25376-200">Tento kód a následné fragmenty pomocí SQL magic tooquery hello ukázka a místní magic tooplot hello data.</span><span class="sxs-lookup"><span data-stu-id="25376-200">This code and subsequent snippets use SQL magic tooquery hello sample and local magic tooplot hello data.</span></span>

* <span data-ttu-id="25376-201">**SQL magic (`%%sql`)** hello jádra HDInsight PySpark podporuje dotazy na snadno vložené HiveQL pro hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="25376-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel supports easy inline HiveQL queries against hello sqlContext.</span></span> <span data-ttu-id="25376-202">Hello (-o VARIABLE_NAME) argument potrvají výstup hello dotazu SQL hello jako Pandas DataFrame na serveru Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="25376-202">hello (-o VARIABLE_NAME) argument persists hello output of hello SQL query as a Pandas DataFrame on hello Jupyter server.</span></span> <span data-ttu-id="25376-203">To znamená, že je k dispozici v místním režimu hello.</span><span class="sxs-lookup"><span data-stu-id="25376-203">This means it is available in hello local mode.</span></span>
* <span data-ttu-id="25376-204">Hello  **`%%local` magic** je použít kód toorun místně na serveru Jupyter hello, což je headnode hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="25376-204">hello **`%%local` magic** is used toorun code locally on hello Jupyter server, which is hello headnode of hello HDInsight cluster.</span></span> <span data-ttu-id="25376-205">Obvykle použijete, `%%local` magic po hello `%%sql -o` magic je použité toorun dotazu.</span><span class="sxs-lookup"><span data-stu-id="25376-205">Typically, you use `%%local` magic after hello `%%sql -o` magic is used toorun a query.</span></span> <span data-ttu-id="25376-206">Parametr -o Hello by zachovat hello výstup hello místně v dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="25376-206">hello -o parameter would persist hello output of hello SQL query locally.</span></span> <span data-ttu-id="25376-207">Potom hello `%%local` magic aktivační události hello další sadu toorun fragmenty kódu místně proti hello výstup hello dotazy SQL, který obsahuje místně trvalé.</span><span class="sxs-lookup"><span data-stu-id="25376-207">Then hello `%%local` magic triggers hello next set of code snippets toorun locally against hello output of hello SQL queries that has been persisted locally.</span></span> <span data-ttu-id="25376-208">výstup Hello se automaticky vizualizuje po spuštění kódu hello.</span><span class="sxs-lookup"><span data-stu-id="25376-208">hello output is automatically visualized after you run hello code.</span></span>

<span data-ttu-id="25376-209">Tento dotaz načte služebních cest hello podle počtu osobní.</span><span class="sxs-lookup"><span data-stu-id="25376-209">This query retrieves hello trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


<span data-ttu-id="25376-210">Tento kód vytvoří místní data snímku z výstupu dotazu hello a zobrazuje hello data.</span><span class="sxs-lookup"><span data-stu-id="25376-210">This code creates a local data-frame from hello query output and plots hello data.</span></span> <span data-ttu-id="25376-211">Hello `%%local` magic vytvoří místní rámce dat, `sqlResults`, který může být použit pro vykreslení s matplotlib.</span><span class="sxs-lookup"><span data-stu-id="25376-211">hello `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="25376-212">Tato PySpark magic se používá více než jednou. v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="25376-212">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="25376-213">Pokud je velká hello množství dat, by měl ukázkové toocreate data rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="25376-213">If hello amount of data is large, you should sample toocreate a data-frame that can fit in local memory.</span></span>
> 
> 

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="25376-214">Zde je osobní hello kód tooplot hello cest počty</span><span class="sxs-lookup"><span data-stu-id="25376-214">Here is hello code tooplot hello trips by passenger counts</span></span>

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

<span data-ttu-id="25376-215">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-215">**OUTPUT**</span></span>

![Frekvence služebních cest podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

<span data-ttu-id="25376-217">Můžete vybrat mezi několika různých typů vizualizace (tabulky, kruhový, řádku, oblasti nebo panelu) pomocí hello **typ** tlačítka nabídky v poznámkovém bloku hello.</span><span class="sxs-lookup"><span data-stu-id="25376-217">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using hello **Type** menu buttons in hello notebook.</span></span> <span data-ttu-id="25376-218">Zobrazí se zde Hello panelu vykreslení.</span><span class="sxs-lookup"><span data-stu-id="25376-218">hello Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="25376-219">Vykreslení histogram tip objemy a jak se liší podle počtu a tarif objemy osobní tip velikost.</span><span class="sxs-lookup"><span data-stu-id="25376-219">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="25376-220">Použít data toosample dotazu SQL...</span><span class="sxs-lookup"><span data-stu-id="25376-220">Use a SQL query toosample data..</span></span>

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


<span data-ttu-id="25376-221">Tuto buňku kód používá hello SQL dotaz toocreate tři pozemků hello data.</span><span class="sxs-lookup"><span data-stu-id="25376-221">This code cell uses hello SQL query toocreate three plots hello data.</span></span>

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


<span data-ttu-id="25376-222">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="25376-222">**OUTPUT:**</span></span> 

![Tip rozdělení částky](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip velikost podle tarif velikost](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="25376-226">Funkce analýzy, transformaci a data přípravy pro modelování</span><span class="sxs-lookup"><span data-stu-id="25376-226">Feature engineering, transformation, and data preparation for modeling</span></span>
<span data-ttu-id="25376-227">Tato část popisuje a zajišťuje, že kód hello postupy použít tooprepare dat pro použití v ML modelování.</span><span class="sxs-lookup"><span data-stu-id="25376-227">This section describes and provides hello code for procedures used tooprepare data for use in ML modeling.</span></span> <span data-ttu-id="25376-228">Ukazuje, jak toodo hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="25376-228">It shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="25376-229">Vytvořte novou funkci tak, že dělení čas do přihrádek doba provozu</span><span class="sxs-lookup"><span data-stu-id="25376-229">Create a new feature by partitioning hours into traffic time bins</span></span>
* <span data-ttu-id="25376-230">Index a na horkou kódování kategorií funkce</span><span class="sxs-lookup"><span data-stu-id="25376-230">Index and on-hot encode categorical features</span></span>
* <span data-ttu-id="25376-231">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="25376-231">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="25376-232">Vytvořit náhodné vzorky dílčí hello dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="25376-232">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
* <span data-ttu-id="25376-233">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="25376-233">Feature scaling</span></span>
* <span data-ttu-id="25376-234">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="25376-234">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a><span data-ttu-id="25376-235">Vytvořte novou funkci tak, že dělení časy provoz do přihrádek</span><span class="sxs-lookup"><span data-stu-id="25376-235">Create a new feature by partitioning traffic times into bins</span></span>
<span data-ttu-id="25376-236">Tento kód ukazuje, jak toocreate novou funkci ve vytváření oddílů provoz časy do přihrádek a poté jak toocache hello výsledné datové rámce v paměti.</span><span class="sxs-lookup"><span data-stu-id="25376-236">This code shows how toocreate a new feature by partitioning traffic times into bins and then how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="25376-237">Ukládání do mezipaměti vede doba provádění tooimproved odolné distribuované datové sady (RDDs) a datových rámců použití opakovaně.</span><span class="sxs-lookup"><span data-stu-id="25376-237">Caching leads tooimproved execution time where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly.</span></span> <span data-ttu-id="25376-238">Ano jsme mezipaměti RDDs a datové rámce v několika fázích v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="25376-238">So, we cache RDDs and data-frames at several stages in this walkthrough.</span></span>

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

<span data-ttu-id="25376-239">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-239">**OUTPUT**</span></span>

<span data-ttu-id="25376-240">126050</span><span class="sxs-lookup"><span data-stu-id="25376-240">126050</span></span>

### <a name="index-and-one-hot-encode-categorical-features"></a><span data-ttu-id="25376-241">Index a jeden horkou kódování kategorií funkce</span><span class="sxs-lookup"><span data-stu-id="25376-241">Index and one-hot encode categorical features</span></span>
<span data-ttu-id="25376-242">Tato část uvádí, jak tooindex nebo kódování kategorií funkce pro vstup do funkce modelování hello.</span><span class="sxs-lookup"><span data-stu-id="25376-242">This section shows how tooindex or encode categorical features for input into hello modeling functions.</span></span> <span data-ttu-id="25376-243">Dobrý den, modelování a předvídání funkce MLlib požadovat, aby se funkce s kategorií vstupní data indexované nebo kódovaný předchozí toouse.</span><span class="sxs-lookup"><span data-stu-id="25376-243">hello modeling and predict functions of MLlib require that features with categorical input data be indexed or encoded prior toouse.</span></span> 

<span data-ttu-id="25376-244">V závislosti na modelu hello potřebovat tooindex nebo zakódovat je různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="25376-244">Depending on hello model, you need tooindex or encode them in different ways.</span></span> <span data-ttu-id="25376-245">Například Logistic a lineární regrese modely vyžadují jeden horkou kódování, kde, například funkce s tří kategorií lze rozšířit na tři sloupce funkce, s každou obsahující 0 nebo 1 v závislosti na kategorii hello pozorování.</span><span class="sxs-lookup"><span data-stu-id="25376-245">For example, Logistic and Linear Regression models require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="25376-246">Poskytuje MLlib [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce toodo horkou jeden kódování.</span><span class="sxs-lookup"><span data-stu-id="25376-246">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function toodo one-hot encoding.</span></span> <span data-ttu-id="25376-247">Tato kodér mapuje sloupec popisek indexy tooa sloupce binární vektorů s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="25376-247">This encoder maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="25376-248">Toto kódování umožňuje algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression funkce toocategorical toobe použít.</span><span class="sxs-lookup"><span data-stu-id="25376-248">This encoding allows algorithms that expect numerical valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

<span data-ttu-id="25376-249">Tady je hello tooindex kódu a kódování kategorií funkce:</span><span class="sxs-lookup"><span data-stu-id="25376-249">Here is hello code tooindex and encode categorical features:</span></span>

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


<span data-ttu-id="25376-250">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-250">**OUTPUT**</span></span>

<span data-ttu-id="25376-251">Doba trvání tooexecute nad buňku: 3.14 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-251">Time taken tooexecute above cell: 3.14 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="25376-252">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="25376-252">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="25376-253">Tato část obsahuje kód, který ukazuje, jak tooindex kategorií textová data jako datový bod s popiskem typ a tooencode ho.</span><span class="sxs-lookup"><span data-stu-id="25376-253">This section contains code that shows how tooindex categorical text data as a labeled point data type and how tooencode it.</span></span> <span data-ttu-id="25376-254">To připraví toobe používá tootrain a testování MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="25376-254">This prepares it toobe used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="25376-255">S popiskem bodu objekty jsou odolné distribuované datové sady (RDD) formátu způsobem, který je nutný jako vstupní data pro většinu ML algoritmy v MLlib.</span><span class="sxs-lookup"><span data-stu-id="25376-255">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="25376-256">A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.</span><span class="sxs-lookup"><span data-stu-id="25376-256">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="25376-257">Tady je hello tooindex kódu a kódování textu funkce pro binární klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="25376-257">Here is hello code tooindex and encode text features for binary classification.</span></span>

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


<span data-ttu-id="25376-258">Tady je kód hello tooencode a index kategorií text funkce pro analýzu lineární regrese.</span><span class="sxs-lookup"><span data-stu-id="25376-258">Here is hello code tooencode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="25376-259">Vytvořit náhodné vzorky dílčí hello dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="25376-259">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
<span data-ttu-id="25376-260">Tento kód vytvoří náhodné vzorky dat hello (25 % tady slouží).</span><span class="sxs-lookup"><span data-stu-id="25376-260">This code creates a random sampling of hello data (25% is used here).</span></span> <span data-ttu-id="25376-261">I když to není nutné v tomto příkladu kvůli toohello velikost hello datovou sadu, ukážeme, jak můžete ukázková data hello sem.</span><span class="sxs-lookup"><span data-stu-id="25376-261">Although it is not required for this example due toohello size of hello dataset, we demonstrate how you can sample hello data here.</span></span> <span data-ttu-id="25376-262">Pak víte, jak toouse pro váš vlastní problém v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="25376-262">Then you know how toouse it for your own problem if needed.</span></span> <span data-ttu-id="25376-263">Po velká vzorky lze ušetřit čas významné při školení modelů.</span><span class="sxs-lookup"><span data-stu-id="25376-263">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="25376-264">Další jsme rozdělit hello ukázka na školení část (v tomto poli 75 %) a testování toouse část (v tomto poli 25 %) v klasifikaci a regresní modelování.</span><span class="sxs-lookup"><span data-stu-id="25376-264">Next we split hello sample into a training part (75% here) and a testing part (25% here) toouse in classification and regression modeling.</span></span>

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

<span data-ttu-id="25376-265">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-265">**OUTPUT**</span></span>

<span data-ttu-id="25376-266">Doba trvání tooexecute nad buňku: 0.31 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-266">Time taken tooexecute above cell: 0.31 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="25376-267">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="25376-267">Feature scaling</span></span>
<span data-ttu-id="25376-268">Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle hello.</span><span class="sxs-lookup"><span data-stu-id="25376-268">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> <span data-ttu-id="25376-269">Hello kódu pro funkce škálování používá hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello funkce toounit odchylky.</span><span class="sxs-lookup"><span data-stu-id="25376-269">hello code for feature scaling uses hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello features toounit variance.</span></span> <span data-ttu-id="25376-270">Pochází od MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD).</span><span class="sxs-lookup"><span data-stu-id="25376-270">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD).</span></span> <span data-ttu-id="25376-271">SGD je Oblíbené algoritmus pro trénování širokou škálu jiných modelů strojového učení například Vyřešeno regresí nebo support vector počítače (SVM).</span><span class="sxs-lookup"><span data-stu-id="25376-271">SGD is a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>   

> [!TIP]
> <span data-ttu-id="25376-272">Našli jsme hello LinearRegressionWithSGD algoritmus toobe citlivé toofeature škálování.</span><span class="sxs-lookup"><span data-stu-id="25376-272">We have found hello LinearRegressionWithSGD algorithm toobe sensitive toofeature scaling.</span></span>   
> 
> 

<span data-ttu-id="25376-273">Zde je hello kód tooscale proměnné pro použití s hello Vyřešeno lineární SGD algoritmus.</span><span class="sxs-lookup"><span data-stu-id="25376-273">Here is hello code tooscale variables for use with hello regularized linear SGD algorithm.</span></span>

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

<span data-ttu-id="25376-274">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-274">**OUTPUT**</span></span>

<span data-ttu-id="25376-275">Doba trvání tooexecute nad buňku: 11.67 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-275">Time taken tooexecute above cell: 11.67 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="25376-276">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="25376-276">Cache objects in memory</span></span>
<span data-ttu-id="25376-277">ukládání do mezipaměti hello vstupní data rámce objekty používá pro klasifikaci, regresi a škálovat funkce může snížit Hello čas potřebný pro trénování a testování ML algoritmů.</span><span class="sxs-lookup"><span data-stu-id="25376-277">hello time taken for training and testing of ML algorithms can be reduced by caching hello input data frame objects used for classification, regression and, scaled features.</span></span>

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

<span data-ttu-id="25376-278">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-278">**OUTPUT**</span></span> 

<span data-ttu-id="25376-279">Doba trvání tooexecute nad buňku: 0,13 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-279">Time taken tooexecute above cell: 0.13 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="25376-280">Předpovědět, zda je tip placené s modely binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="25376-280">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="25376-281">Tato část uvádí, jak použití, tři modely pro predikci hello binární klasifikace úlohy zda tip je placené taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="25376-281">This section shows how use three models for hello binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="25376-282">modely Hello uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="25376-282">hello models presented are:</span></span>

* <span data-ttu-id="25376-283">Logistic regression</span><span class="sxs-lookup"><span data-stu-id="25376-283">Logistic regression</span></span> 
* <span data-ttu-id="25376-284">Náhodné doménové struktury</span><span class="sxs-lookup"><span data-stu-id="25376-284">Random forest</span></span>
* <span data-ttu-id="25376-285">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="25376-285">Gradient Boosting Trees</span></span>

<span data-ttu-id="25376-286">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="25376-286">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="25376-287">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="25376-287">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="25376-288">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="25376-288">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="25376-289">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="25376-289">**Saving model** in blob for future consumption</span></span>

<span data-ttu-id="25376-290">Ukážeme, jak toodo křížové ověření (odchylka nákladů) s parametrem komínů dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="25376-290">We show how toodo cross-validation (CV) with parameter sweeping in two ways:</span></span>

1. <span data-ttu-id="25376-291">Pomocí **Obecné** vlastní kód, který může být použité tooany algoritmus MLlib a tooany parametr nastaví v algoritmu.</span><span class="sxs-lookup"><span data-stu-id="25376-291">Using **generic** custom code which can be applied tooany algorithm in MLlib and tooany parameter sets in an algorithm.</span></span> 
2. <span data-ttu-id="25376-292">Pomocí hello **pySpark CrossValidator kanálu funkce**.</span><span class="sxs-lookup"><span data-stu-id="25376-292">Using hello **pySpark CrossValidator pipeline function**.</span></span> <span data-ttu-id="25376-293">Všimněte si, že CrossValidator má několik omezení pro Spark 1.5.0:</span><span class="sxs-lookup"><span data-stu-id="25376-293">Note that CrossValidator has a few limitations for Spark 1.5.0:</span></span> 
   
   * <span data-ttu-id="25376-294">Modely kanálu nelze uložit nebo trvalá pro budoucí spotřeby.</span><span class="sxs-lookup"><span data-stu-id="25376-294">Pipeline models cannot be saved/persisted for future consumption.</span></span>
   * <span data-ttu-id="25376-295">Nelze použít pro každý parametr v modelu.</span><span class="sxs-lookup"><span data-stu-id="25376-295">Cannot be used for every parameter in a model.</span></span>
   * <span data-ttu-id="25376-296">Nelze použít pro každý MLlib algoritmus.</span><span class="sxs-lookup"><span data-stu-id="25376-296">Cannot be used for every MLlib algorithm.</span></span>

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-hello-logistic-regression-algorithm-for-binary-classification"></a><span data-ttu-id="25376-297">Obecná křížové ověření a hyperparameter (vymetání) komínů použít s hello logistic regresní algoritmus pro binární klasifikaci</span><span class="sxs-lookup"><span data-stu-id="25376-297">Generic cross validation and hyperparameter sweeping used with hello logistic regression algorithm for binary classification</span></span>
<span data-ttu-id="25376-298">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="25376-298">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="25376-299">cvičení modelu Hello pomocí křížové ověření (odchylka nákladů) a hyperparameter (vymetání) komínů implementuje pomocí vlastní kód, který může být použité tooany hello učení algoritmy v MLlib.</span><span class="sxs-lookup"><span data-stu-id="25376-299">hello model is trained using cross validation (CV) and hyperparameter sweeping implemented with custom code that can be applied tooany of hello learning algorithms in MLlib.</span></span>   

> [!NOTE]
> <span data-ttu-id="25376-300">Hello provádění tohoto vlastního kódu odchylka nákladů může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="25376-300">hello execution of this custom CV code can take several minutes.</span></span>
> 
> 

<span data-ttu-id="25376-301">**Cvičení hello logistic regresní model pomocí odchylka nákladů a hyperparameter (vymetání) komínů**</span><span class="sxs-lookup"><span data-stu-id="25376-301">**Train hello logistic regression model using CV and hyperparameter sweeping**</span></span>

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


<span data-ttu-id="25376-302">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-302">**OUTPUT**</span></span>

<span data-ttu-id="25376-303">Koeficienty: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="25376-303">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="25376-304">Zachycení:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="25376-304">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="25376-305">Doba trvání tooexecute nad buňku: 14.43 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-305">Time taken tooexecute above cell: 14.43 seconds</span></span>

<span data-ttu-id="25376-306">**Vyhodnocení modelu binární klasifikace hello o standardní metriky**</span><span class="sxs-lookup"><span data-stu-id="25376-306">**Evaluate hello binary classification model with standard metrics**</span></span>

<span data-ttu-id="25376-307">Hello kód v této části ukazuje, jak tooevaluate logistic regresní model proti testovací data sada, včetně vykreslení hello: křivka ROC.</span><span class="sxs-lookup"><span data-stu-id="25376-307">hello code in this section shows how tooevaluate a logistic regression model against a test data-set, including a plot of hello ROC curve.</span></span>

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


<span data-ttu-id="25376-308">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-308">**OUTPUT**</span></span>

<span data-ttu-id="25376-309">Oblasti v rámci PR = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="25376-309">Area under PR = 0.985336538462</span></span>

<span data-ttu-id="25376-310">Oblasti v rámci ROC = 0.983383274312</span><span class="sxs-lookup"><span data-stu-id="25376-310">Area under ROC = 0.983383274312</span></span>

<span data-ttu-id="25376-311">Souhrnné statistiky</span><span class="sxs-lookup"><span data-stu-id="25376-311">Summary Stats</span></span>

<span data-ttu-id="25376-312">Přesnost = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="25376-312">Precision = 0.984174341679</span></span>

<span data-ttu-id="25376-313">Odvolat = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="25376-313">Recall = 0.984174341679</span></span>

<span data-ttu-id="25376-314">F1 Stanovení skóre = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="25376-314">F1 Score = 0.984174341679</span></span>

<span data-ttu-id="25376-315">Doba trvání tooexecute nad buňku: 2.67 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-315">Time taken tooexecute above cell: 2.67 seconds</span></span>

<span data-ttu-id="25376-316">**Vykreslení křivka ROC hello.**</span><span class="sxs-lookup"><span data-stu-id="25376-316">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="25376-317">Hello *predictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku hello.</span><span class="sxs-lookup"><span data-stu-id="25376-317">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="25376-318">*tmp_results* je možné použít toodo dotazy a výstup výsledků do hello sqlResults dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="25376-318">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="25376-319">Tady je kód hello.</span><span class="sxs-lookup"><span data-stu-id="25376-319">Here is hello code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="25376-320">Zde je hello kód toomake předpovědi a vykreslení hello křivka ROC.</span><span class="sxs-lookup"><span data-stu-id="25376-320">Here is hello code toomake predictions and plot hello ROC-curve.</span></span>

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


<span data-ttu-id="25376-321">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-321">**OUTPUT**</span></span>

![Křivka ROC logistic regression pro obecný přístup](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

<span data-ttu-id="25376-323">**Zachovat modelu do objektu BLOB pro budoucí spotřeba**</span><span class="sxs-lookup"><span data-stu-id="25376-323">**Persist model in a blob for future consumption**</span></span>

<span data-ttu-id="25376-324">Hello kód v této části ukazuje, jak toosave hello logistic regresní model pro používání.</span><span class="sxs-lookup"><span data-stu-id="25376-324">hello code in this section shows how toosave hello logistic regression model for consumption.</span></span>

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


<span data-ttu-id="25376-325">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-325">**OUTPUT**</span></span>

<span data-ttu-id="25376-326">Doba trvání tooexecute nad buňku: 34.57 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-326">Time taken tooexecute above cell: 34.57 seconds</span></span>

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a><span data-ttu-id="25376-327">Pomocí funkce kanálu CrossValidator na MLlib s modelem logistic regression (elastické regrese)</span><span class="sxs-lookup"><span data-stu-id="25376-327">Use MLlib's CrossValidator pipeline function with logistic regression (Elastic regression) model</span></span>
<span data-ttu-id="25376-328">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="25376-328">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="25376-329">cvičení modelu Hello pomocí křížového ověření (odchylka nákladů) a hyperparameter (vymetání) komínů implementováno s hello MLlib CrossValidator kanálu funkce pro odchylka nákladů s oblouku parametr.</span><span class="sxs-lookup"><span data-stu-id="25376-329">hello model is trained using cross validation (CV) and hyperparameter sweeping implemented with hello MLlib CrossValidator pipeline function for CV with parameter sweep.</span></span>   

> [!NOTE]
> <span data-ttu-id="25376-330">Hello provádění tohoto kódu odchylka MLlib nákladů může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="25376-330">hello execution of this MLlib CV code can take several minutes.</span></span>
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

<span data-ttu-id="25376-331">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-331">**OUTPUT**</span></span>

<span data-ttu-id="25376-332">Doba trvání tooexecute nad buňku: 107.98 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-332">Time taken tooexecute above cell: 107.98 seconds</span></span>

<span data-ttu-id="25376-333">**Vykreslení křivka ROC hello.**</span><span class="sxs-lookup"><span data-stu-id="25376-333">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="25376-334">Hello *predictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku hello.</span><span class="sxs-lookup"><span data-stu-id="25376-334">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="25376-335">*tmp_results* je možné použít toodo dotazy a výstup výsledků do hello sqlResults dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="25376-335">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="25376-336">Tady je kód hello.</span><span class="sxs-lookup"><span data-stu-id="25376-336">Here is hello code.</span></span>

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

<span data-ttu-id="25376-337">Zde je kód hello tooplot: křivka ROC hello.</span><span class="sxs-lookup"><span data-stu-id="25376-337">Here is hello code tooplot hello ROC curve.</span></span>

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


<span data-ttu-id="25376-338">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-338">**OUTPUT**</span></span>

![Pomocí CrossValidator na MLlib křivka ROC logistic regression](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="25376-340">Klasifikace náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="25376-340">Random forest classification</span></span>
<span data-ttu-id="25376-341">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit regrese náhodných doménové struktury, který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="25376-341">hello code in this section shows how tootrain, evaluate, and save a random forest regression that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

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


<span data-ttu-id="25376-342">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-342">**OUTPUT**</span></span>

<span data-ttu-id="25376-343">Oblasti v rámci ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="25376-343">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="25376-344">Doba trvání tooexecute nad buňku: 26.72 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-344">Time taken tooexecute above cell: 26.72 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="25376-345">Přechodu zvýšení skóre klasifikace stromů</span><span class="sxs-lookup"><span data-stu-id="25376-345">Gradient boosting trees classification</span></span>
<span data-ttu-id="25376-346">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá, zda je tip zaplacení cesty v hello NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="25376-346">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

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

<span data-ttu-id="25376-347">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-347">**OUTPUT**</span></span>

<span data-ttu-id="25376-348">Oblasti v rámci ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="25376-348">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="25376-349">Doba trvání tooexecute nad buňku: 28.13 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-349">Time taken tooexecute above cell: 28.13 seconds</span></span>

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a><span data-ttu-id="25376-350">Předpověď tip velikost s modely regrese (nepoužíváte odchylka nákladů)</span><span class="sxs-lookup"><span data-stu-id="25376-350">Predict tip amount with regression models (not using CV)</span></span>
<span data-ttu-id="25376-351">V této části ukazuje, jak použít, tři modely pro úlohy regrese hello: předpovědi velikost tip hello placené taxíkem cesty na základě jiných funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="25376-351">This section shows how use three models for hello regression task: predict hello tip amount paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="25376-352">modely Hello uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="25376-352">hello models presented are:</span></span>

* <span data-ttu-id="25376-353">Vyřešeno lineární regrese</span><span class="sxs-lookup"><span data-stu-id="25376-353">Regularized linear regression</span></span>
* <span data-ttu-id="25376-354">Náhodné doménové struktury</span><span class="sxs-lookup"><span data-stu-id="25376-354">Random forest</span></span>
* <span data-ttu-id="25376-355">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="25376-355">Gradient Boosting Trees</span></span>

<span data-ttu-id="25376-356">Tyto modely byly popsané v úvodu hello.</span><span class="sxs-lookup"><span data-stu-id="25376-356">These models were described in hello introduction.</span></span> <span data-ttu-id="25376-357">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="25376-357">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="25376-358">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="25376-358">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="25376-359">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="25376-359">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="25376-360">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="25376-360">**Saving model** in blob for future consumption</span></span>   

> <span data-ttu-id="25376-361">AZURE Poznámka: Křížové ověření není použít s hello tři modely Regrese v této části, protože to byla vidět v souvislosti s modely logistic regression hello.</span><span class="sxs-lookup"><span data-stu-id="25376-361">AZURE NOTE: Cross-validation is not used with hello three regression models in this section, since this was shown in detail for hello logistic regression models.</span></span> <span data-ttu-id="25376-362">Příklad znázorňující, jak je uvedený toouse odchylka nákladů s elastické Net pro lineární regrese v hello příloha tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="25376-362">An example showing how toouse CV with Elastic Net for linear regression is provided in hello Appendix of this topic.</span></span>
> 
> <span data-ttu-id="25376-363">AZURE Poznámka: V našich zkušeností, může být problémy s konvergence LinearRegressionWithSGD modelů a parametry potřebovat toobe změnit/optimalizované pečlivě pro získání platný model.</span><span class="sxs-lookup"><span data-stu-id="25376-363">AZURE NOTE: In our experience, there can be issues with convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="25376-364">Škálování proměnných výrazně pomáhá s konvergence.</span><span class="sxs-lookup"><span data-stu-id="25376-364">Scaling of variables significantly helps with convergence.</span></span> <span data-ttu-id="25376-365">Elastické net regrese uvedené v tématu toothis hello příloha, mohou sloužit také místo LinearRegressionWithSGD.</span><span class="sxs-lookup"><span data-stu-id="25376-365">Elastic net regression, shown in hello Appendix toothis topic, can also be used instead of LinearRegressionWithSGD.</span></span>
> 
> 

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="25376-366">Lineární regrese s SGD</span><span class="sxs-lookup"><span data-stu-id="25376-366">Linear regression with SGD</span></span>
<span data-ttu-id="25376-367">Hello kód v této části ukazuje, jak toouse škálovat funkce tootrain lineární regrese, který stochastického přechodu klesání (SGD) používá pro optimalizaci, a jak tooscore, hodnocení a uložit hello model v Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="25376-367">hello code in this section shows how toouse scaled features tootrain a linear regression that uses stochastic gradient descent (SGD) for optimization, and how tooscore, evaluate, and save hello model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="25376-368">V našich zkušeností může být problémy s hello konvergence LinearRegressionWithSGD modelů a parametry potřebovat toobe změnit/optimalizované pečlivě pro získání platný model.</span><span class="sxs-lookup"><span data-stu-id="25376-368">In our experience, there can be issues with hello convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="25376-369">Škálování proměnných výrazně pomáhá s konvergence.</span><span class="sxs-lookup"><span data-stu-id="25376-369">Scaling of variables significantly helps with convergence.</span></span>
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

<span data-ttu-id="25376-370">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-370">**OUTPUT**</span></span>

<span data-ttu-id="25376-371">Koeficienty: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]</span><span class="sxs-lookup"><span data-stu-id="25376-371">Coefficients: [0.0141707753435, -0.0252930927087, -0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092, -0.00456498588241, -0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632, -0.00289545676449, -0.00791124681938, 0.54396316518, -0.536293513569, 0.0119076553369, -0.0173039244582, 0.0119632796147, 0.00146764882502]</span></span>

<span data-ttu-id="25376-372">Zachytávat: 0.854507624459</span><span class="sxs-lookup"><span data-stu-id="25376-372">Intercept: 0.854507624459</span></span>

<span data-ttu-id="25376-373">RMSE = 1.23485131376</span><span class="sxs-lookup"><span data-stu-id="25376-373">RMSE = 1.23485131376</span></span>

<span data-ttu-id="25376-374">R sqr = 0.597963951127</span><span class="sxs-lookup"><span data-stu-id="25376-374">R-sqr = 0.597963951127</span></span>

<span data-ttu-id="25376-375">Doba trvání tooexecute nad buňku: 38.62 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-375">Time taken tooexecute above cell: 38.62 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="25376-376">Regrese náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="25376-376">Random Forest regression</span></span>
<span data-ttu-id="25376-377">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit model náhodných doménové struktury, který předpovídá velikost tip pro hello NYC taxíkem cestě data.</span><span class="sxs-lookup"><span data-stu-id="25376-377">hello code in this section shows how tootrain, evaluate, and save a random forest model that predicts tip amount for hello NYC taxi trip data.</span></span>   

> [!NOTE]
> <span data-ttu-id="25376-378">Křížové ověření s parametrem komínů pomocí vlastního kódu jsou uvedené v dodatku hello.</span><span class="sxs-lookup"><span data-stu-id="25376-378">Cross-validation with parameter sweeping using custom code is provided in hello appendix.</span></span>
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

<span data-ttu-id="25376-379">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-379">**OUTPUT**</span></span>

<span data-ttu-id="25376-380">RMSE = 0.931981967875</span><span class="sxs-lookup"><span data-stu-id="25376-380">RMSE = 0.931981967875</span></span>

<span data-ttu-id="25376-381">R sqr = 0.733445485802</span><span class="sxs-lookup"><span data-stu-id="25376-381">R-sqr = 0.733445485802</span></span>

<span data-ttu-id="25376-382">Doba trvání tooexecute nad buňku: 25.98 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-382">Time taken tooexecute above cell: 25.98 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="25376-383">Přechodu zvýšení skóre regresní stromy</span><span class="sxs-lookup"><span data-stu-id="25376-383">Gradient boosting trees regression</span></span>
<span data-ttu-id="25376-384">Hello kód v této části ukazuje, jak tootrain, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá velikost tip pro hello NYC taxíkem cestě data.</span><span class="sxs-lookup"><span data-stu-id="25376-384">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts tip amount for hello NYC taxi trip data.</span></span>

<span data-ttu-id="25376-385">** Natrénování a vyhodnocení **</span><span class="sxs-lookup"><span data-stu-id="25376-385">**Train and evaluate **</span></span>

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


<span data-ttu-id="25376-386">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-386">**OUTPUT**</span></span>

<span data-ttu-id="25376-387">RMSE = 0.928172197114</span><span class="sxs-lookup"><span data-stu-id="25376-387">RMSE = 0.928172197114</span></span>

<span data-ttu-id="25376-388">R sqr = 0.732680354389</span><span class="sxs-lookup"><span data-stu-id="25376-388">R-sqr = 0.732680354389</span></span>

<span data-ttu-id="25376-389">Doba trvání tooexecute nad buňku: 20.9 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-389">Time taken tooexecute above cell: 20.9 seconds</span></span>

<span data-ttu-id="25376-390">**Vykreslení.**</span><span class="sxs-lookup"><span data-stu-id="25376-390">**Plot**</span></span>

<span data-ttu-id="25376-391">*tmp_results* je registrován jako tabulku Hive v předchozí buňku hello.</span><span class="sxs-lookup"><span data-stu-id="25376-391">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="25376-392">Výsledky z tabulky hello jsou výstupem do hello *sqlResults* dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="25376-392">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="25376-393">Tady je kód hello</span><span class="sxs-lookup"><span data-stu-id="25376-393">Here is hello code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="25376-394">Zde je hello kód tooplot hello dat pomocí hello Jupyter server.</span><span class="sxs-lookup"><span data-stu-id="25376-394">Here is hello code tooplot hello data using hello Jupyter server.</span></span>

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

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a><span data-ttu-id="25376-396">Dodatek: Další regresní úlohy křížového ověření pomocí parametru změny</span><span class="sxs-lookup"><span data-stu-id="25376-396">Appendix: Additional regression tasks using cross validation with parameter sweeps</span></span>
<span data-ttu-id="25376-397">Tento dodatek obsahuje kód znázorňující způsob toodo odchylka nákladů pomocí elastické net pro lineární regrese a jak toodo odchylka nákladů s parametrem oblouku pomocí vlastní kód pro regresní náhodných doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="25376-397">This appendix contains code showing how toodo CV using Elastic net for linear regression and how toodo CV with parameter sweep using custom code for random forest regression.</span></span>

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a><span data-ttu-id="25376-398">Křížové ověření pomocí Elastická net pro lineární regrese</span><span class="sxs-lookup"><span data-stu-id="25376-398">Cross validation using Elastic net for linear regression</span></span>
<span data-ttu-id="25376-399">Hello kód v této části ukazuje, jak toodo křížové ověření pomocí Elastická net pro lineární regrese a jak tooevaluate hello model testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="25376-399">hello code in this section shows how toodo cross validation using Elastic net for linear regression and how tooevaluate hello model against test data.</span></span>

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


<span data-ttu-id="25376-400">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-400">**OUTPUT**</span></span>

<span data-ttu-id="25376-401">Doba trvání tooexecute nad buňku: 161.21 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-401">Time taken tooexecute above cell: 161.21  seconds</span></span>

<span data-ttu-id="25376-402">**Vyhodnocení s metrikou R SQR**</span><span class="sxs-lookup"><span data-stu-id="25376-402">**Evaluate with R-SQR metric**</span></span>

<span data-ttu-id="25376-403">*tmp_results* je registrován jako tabulku Hive v předchozí buňku hello.</span><span class="sxs-lookup"><span data-stu-id="25376-403">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="25376-404">Výsledky z tabulky hello jsou výstupem do hello *sqlResults* dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="25376-404">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="25376-405">Tady je kód hello</span><span class="sxs-lookup"><span data-stu-id="25376-405">Here is hello code</span></span>

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


<span data-ttu-id="25376-406">Zde je kód toocalculate hello R sqr.</span><span class="sxs-lookup"><span data-stu-id="25376-406">Here is hello code toocalculate R-sqr.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


<span data-ttu-id="25376-407">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-407">**OUTPUT**</span></span>

<span data-ttu-id="25376-408">R sqr = 0.619184907088</span><span class="sxs-lookup"><span data-stu-id="25376-408">R-sqr = 0.619184907088</span></span>

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a><span data-ttu-id="25376-409">Křížové ověření pomocí parametru oblouku pomocí vlastní kód pro regresní náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="25376-409">Cross validation with parameter sweep using custom code for random forest regression</span></span>
<span data-ttu-id="25376-410">Hello kód v této části ukazuje, jak toodo křížové ověření pomocí parametru oblouku pomocí vlastní kód pro regresní náhodných doménové struktury a jak tooevaluate hello model testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="25376-410">hello code in this section shows how toodo cross validation with parameter sweep using custom code for random forest regression and how tooevaluate hello model against test data.</span></span>

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


<span data-ttu-id="25376-411">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-411">**OUTPUT**</span></span>

<span data-ttu-id="25376-412">RMSE = 0.906972198262</span><span class="sxs-lookup"><span data-stu-id="25376-412">RMSE = 0.906972198262</span></span>

<span data-ttu-id="25376-413">R sqr = 0.740751197012</span><span class="sxs-lookup"><span data-stu-id="25376-413">R-sqr = 0.740751197012</span></span>

<span data-ttu-id="25376-414">Doba trvání tooexecute nad buňku: 69.17 sekund</span><span class="sxs-lookup"><span data-stu-id="25376-414">Time taken tooexecute above cell: 69.17 seconds</span></span>

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a><span data-ttu-id="25376-415">Vyčištění objektů z paměti a umístění tiskové modelu</span><span class="sxs-lookup"><span data-stu-id="25376-415">Clean up objects from memory and print model locations</span></span>
<span data-ttu-id="25376-416">Použití `unpersist()` toodelete objekty uložené v mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="25376-416">Use `unpersist()` toodelete objects cached in memory.</span></span>

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


<span data-ttu-id="25376-417">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-417">**OUTPUT**</span></span>

<span data-ttu-id="25376-418">PythonRDD [122] v RDD v PythonRDD.scala: 43</span><span class="sxs-lookup"><span data-stu-id="25376-418">PythonRDD[122] at RDD at PythonRDD.scala: 43</span></span>

<span data-ttu-id="25376-419">** Výtisk cesta toomodel soubory toobe použít v poznámkovém bloku spotřeba hello.</span><span class="sxs-lookup"><span data-stu-id="25376-419">**Printout path toomodel files toobe used in hello consumption notebook.</span></span> <span data-ttu-id="25376-420">** tooconsume a skóre nezávislou data-set, potřebujete toocopy a vložte tyto názvy souborů hello "Spotřeba poznámkového bloku".</span><span class="sxs-lookup"><span data-stu-id="25376-420">** tooconsume and score an independent data-set, you need toocopy and paste these file names in hello "Consumption notebook".</span></span>

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="25376-421">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="25376-421">**OUTPUT**</span></span>

<span data-ttu-id="25376-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span><span class="sxs-lookup"><span data-stu-id="25376-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span></span>

<span data-ttu-id="25376-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span><span class="sxs-lookup"><span data-stu-id="25376-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span></span>

<span data-ttu-id="25376-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span><span class="sxs-lookup"><span data-stu-id="25376-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span></span>

<span data-ttu-id="25376-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span><span class="sxs-lookup"><span data-stu-id="25376-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span></span>

<span data-ttu-id="25376-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span><span class="sxs-lookup"><span data-stu-id="25376-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span></span>

<span data-ttu-id="25376-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span><span class="sxs-lookup"><span data-stu-id="25376-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span></span>

## <a name="whats-next"></a><span data-ttu-id="25376-428">Co dále?</span><span class="sxs-lookup"><span data-stu-id="25376-428">What's next?</span></span>
<span data-ttu-id="25376-429">Teď, když jste vytvořili regrese a klasifikace modely s hello Spark MlLib, jak jste připravené toolearn tooscore a vyhodnocovat u nich těchto modelů.</span><span class="sxs-lookup"><span data-stu-id="25376-429">Now that you have created regression and classification models with hello Spark MlLib, you are ready toolearn how tooscore and evaluate these models.</span></span>

<span data-ttu-id="25376-430">**Model spotřeby:** toolearn jak tooscore a vyhodnotit hello klasifikace a regrese modelů vytvořených v tomto tématu najdete v tématu [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="25376-430">**Model consumption:** toolearn how tooscore and evaluate hello classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

