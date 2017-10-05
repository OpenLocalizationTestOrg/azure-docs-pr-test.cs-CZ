---
title: "Pokročilé zkoumání dat a modelování pomocí Spark | Microsoft Docs"
description: "Pomocí HDInsight Spark proveďte zkoumání dat a cvičení binární klasifikace a regrese modelů pomocí křížové ověření a hyperparameter optimalizace."
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
ms.openlocfilehash: e6bf6bd3c905f077841ef166540337a251b91ad1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a><span data-ttu-id="6dda9-103">Pokročilé zkoumání a modelování dat pomocí Spark</span><span class="sxs-lookup"><span data-stu-id="6dda9-103">Advanced data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="6dda9-104">Tento návod používá HDInsight Spark uděláte zkoumání dat a cvičení binární klasifikace a modely regrese pomocí křížové ověření a hyperparameter optimalizace na základě vzorku NYC taxi cestě a jízdenky 2013 datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-104">This walkthrough uses HDInsight Spark to do data exploration and train binary classification and regression models using cross-validation and hyperparameter optimization on a sample of the NYC taxi trip and fare 2013 dataset.</span></span> <span data-ttu-id="6dda9-105">Provede vás provede postupem [proces vědecké účely dat](http://aka.ms/datascienceprocess), klient server, pomocí clusteru služby HDInsight Spark pro zpracování a Azure objektů BLOB k ukládání dat a modely.</span><span class="sxs-lookup"><span data-stu-id="6dda9-105">It walks you through the steps of the [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs to store the data and the models.</span></span> <span data-ttu-id="6dda9-106">Proces jsou zde popsány vizualizuje dat získaných z objektu Blob úložiště Azure a pak připraví dat za účelem vytvoření prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="6dda9-106">The process explores and visualizes data brought in from an Azure Storage Blob and then prepares the data to build predictive models.</span></span> <span data-ttu-id="6dda9-107">Python byla použita k code řešení a zobrazíte relevantní pozemků.</span><span class="sxs-lookup"><span data-stu-id="6dda9-107">Python has been used to code the solution and to show the relevant plots.</span></span> <span data-ttu-id="6dda9-108">Tyto modely jsou sestavení pomocí sady nástrojů pro Spark MLlib provádění binární klasifikaci a regresní modelování úkolů.</span><span class="sxs-lookup"><span data-stu-id="6dda9-108">These models are build using the Spark MLlib toolkit to do binary classification and regression modeling tasks.</span></span> 

* <span data-ttu-id="6dda9-109">**Binární klasifikace** úloh je k předvídání, zda je tip placené pro cestu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-109">The **binary classification** task is to predict whether or not a tip is paid for the trip.</span></span> 
* <span data-ttu-id="6dda9-110">**Regrese** úloh je k předvídání množství tip podle dalších funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="6dda9-110">The **regression** task is to predict the amount of the tip based on other tip features.</span></span> 

<span data-ttu-id="6dda9-111">Modelování kroky také obsahovat kód znázorňující cvičení, hodnocení a uložit každý typ modelu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-111">The modeling steps also contain code showing how to train, evaluate, and save each type of model.</span></span> <span data-ttu-id="6dda9-112">Téma pokrývá některé stejné zemi jako [zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-data-exploration-modeling.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-112">The topic covers some of the same ground as the [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic.</span></span> <span data-ttu-id="6dda9-113">Ale ho je "složitější," také používá křížového ověřování s hyperparameter komínů ke cvičení optimálně přesné klasifikace a regrese modelů.</span><span class="sxs-lookup"><span data-stu-id="6dda9-113">But it is more "advanced" in that it also uses cross-validation with hyperparameter sweeping to train optimally accurate classification and regression models.</span></span> 

<span data-ttu-id="6dda9-114">**Křížové ověření (odchylka nákladů)** je technika, který vyhodnocuje, jak dobře model trénink na známé sadu dat umožňuje zobecnit pro predikci funkcí datové sady, na kterém je nebyla cvičena.</span><span class="sxs-lookup"><span data-stu-id="6dda9-114">**Cross-validation (CV)** is a technique that assesses how well a model trained on a known set of data generalizes to predicting the features of datasets on which it has not been trained.</span></span>  <span data-ttu-id="6dda9-115">Běžná implementace použít zde je k rozdělení datové sady na tisíc složení a pak trénování modelu v kruhového dotazování na všechny kromě jednoho složení.</span><span class="sxs-lookup"><span data-stu-id="6dda9-115">A common implementation used here is to divide a dataset into K folds and then train the model in a round-robin fashion on all but one of the folds.</span></span> <span data-ttu-id="6dda9-116">Se hodnotí schopnost modelu, který má předpovědi přesně, při testování proti nezávislé datovou sadu v této násobek nejsou používány k trénování modelu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-116">The ability of the model to prediction accurately when tested against the independent dataset in this fold not used to train the model is assessed.</span></span>

<span data-ttu-id="6dda9-117">**Optimalizace Hyperparameter** je tento problém vybrat sadu hyperparameters pro algoritmu učení, obvykle s cílem optimalizace měření výkonu algoritmus na nezávislé datové sady.</span><span class="sxs-lookup"><span data-stu-id="6dda9-117">**Hyperparameter optimization** is the problem of choosing a set of hyperparameters for a learning algorithm, usually with the goal of optimizing a measure of the algorithm's performance on an independent data set.</span></span> <span data-ttu-id="6dda9-118">**Hyperparameters** jsou hodnoty, které musí být zadán mimo proces školení modelu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-118">**Hyperparameters** are values that must be specified outside of the model training procedure.</span></span> <span data-ttu-id="6dda9-119">Předpoklady o tyto hodnoty může ovlivnit flexibilitu a přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-119">Assumptions about these values can impact the flexibility and accuracy of the models.</span></span> <span data-ttu-id="6dda9-120">Rozhodovací stromy mít hyperparameters, například jako je například požadovaný hloubkou a počet nechá ve stromové struktuře.</span><span class="sxs-lookup"><span data-stu-id="6dda9-120">Decision trees have hyperparameters, for example, such as the desired depth and number of leaves in the tree.</span></span> <span data-ttu-id="6dda9-121">Podpora vektoru počítače (SVMs) vyžadují, nastavení termín snížení chybnou klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="6dda9-121">Support Vector Machines (SVMs) require setting a misclassification penalty term.</span></span> 

<span data-ttu-id="6dda9-122">Běžný způsob provedení optimalizace hyperparameter použít zde je hledání mřížky nebo **oblouku parametr**.</span><span class="sxs-lookup"><span data-stu-id="6dda9-122">A common way to perform hyperparameter optimization used here is a grid search, or a **parameter sweep**.</span></span> <span data-ttu-id="6dda9-123">Tento postup se skládá provádět důkladné prohledání prostřednictvím hodnoty podmnožinu místo hyperparameter zadaný pro algoritmus učení.</span><span class="sxs-lookup"><span data-stu-id="6dda9-123">This consists of performing an exhaustive search through the values a specified subset of the hyperparameter space for a learning algorithm.</span></span> <span data-ttu-id="6dda9-124">Křížového ověření může poskytovat metriky výkonu seřadit optimální výsledky vyprodukované vyhledávací algoritmus mřížky.</span><span class="sxs-lookup"><span data-stu-id="6dda9-124">Cross validation can supply a performance metric to sort out the optimal results produced by the grid search algorithm.</span></span> <span data-ttu-id="6dda9-125">Odchylka nákladů použít s obezřetností pomáhá hyperparameter limit problémy jako overfitting modelu pro Cvičná data tak, aby model uchovává kapacity, které chcete použít pro obecné sadu dat, ze kterého jste extrahovali Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="6dda9-125">CV used with hyperparameter sweeping helps limit problems like overfitting a model to training data so that the model retains the capacity to apply to the general set of data from which the training data was extracted.</span></span>

<span data-ttu-id="6dda9-126">Modely, které používáme zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromů:</span><span class="sxs-lookup"><span data-stu-id="6dda9-126">The models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="6dda9-127">[Lineární regrese s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je model lineární regrese, který používá metodu Stochastického přechodu klesání (SGD) a škálování k předvídání objemy tip placené pro optimalizaci a funkce.</span><span class="sxs-lookup"><span data-stu-id="6dda9-127">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling to predict the tip amounts paid.</span></span> 
* <span data-ttu-id="6dda9-128">[Logistic regression s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) nebo "logit" regression je regresní model, který lze použít při kategorií udělat klasifikace dat je závislé proměnné.</span><span class="sxs-lookup"><span data-stu-id="6dda9-128">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when the dependent variable is categorical to do data classification.</span></span> <span data-ttu-id="6dda9-129">LBFGS je algoritmus optimalizace jako Newton, blíží algoritmus Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomocí omezené množství paměti počítače a který se často používá v machine learning.</span><span class="sxs-lookup"><span data-stu-id="6dda9-129">LBFGS is a quasi-Newton optimization algorithm that approximates the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="6dda9-130">[Náhodné doménových strukturách](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="6dda9-130">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="6dda9-131">Že kombinují mnoho rozhodovacích stromů, aby se snížilo riziko overfitting.</span><span class="sxs-lookup"><span data-stu-id="6dda9-131">They combine many decision trees to reduce the risk of overfitting.</span></span> <span data-ttu-id="6dda9-132">Náhodné doménovými strukturami se používají pro regresní a klasifikaci a dokáže zpracovat kategorií funkce a lze rozšířit pro nastavení více třídami klasifikace.</span><span class="sxs-lookup"><span data-stu-id="6dda9-132">Random forests are used for regression and classification and can handle categorical features and can be extended to the multiclass classification setting.</span></span> <span data-ttu-id="6dda9-133">Tyto nevyžadují funkce škálování a mohli zaznamenat nelineárností a funkci interakce.</span><span class="sxs-lookup"><span data-stu-id="6dda9-133">They do not require feature scaling and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="6dda9-134">Náhodné doménových strukturách jsou jedním z těch nejúspěšnějších strojového učení modely pro klasifikaci a regrese.</span><span class="sxs-lookup"><span data-stu-id="6dda9-134">Random forests are one of the most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="6dda9-135">[Přechodu boosted stromy](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) jsou komplety rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="6dda9-135">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="6dda9-136">GBTs cvičení stromů rozhodnutí interaktivně, aby se minimalizoval funkci ztrátu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-136">GBTs train decision trees iteratively to minimize a loss function.</span></span> <span data-ttu-id="6dda9-137">GBTs se používají pro regresní a klasifikaci a dokáže zpracovat kategorií funkce, nevyžadují funkce škálování a mohli zaznamenat nelineárností a funkci interakce.</span><span class="sxs-lookup"><span data-stu-id="6dda9-137">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="6dda9-138">Můžete také používají v nastavení multiclass klasifikace.</span><span class="sxs-lookup"><span data-stu-id="6dda9-138">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="6dda9-139">Modelování příklady použití odchylka nákladů a Hyperparameter oblouku se zobrazují pro problém binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="6dda9-139">Modeling examples using CV and Hyperparameter sweep are shown for the binary classification problem.</span></span> <span data-ttu-id="6dda9-140">Jednodušší příklady (bez parametru přesune) jsou uvedeny na hlavní téma pro regresní úlohy.</span><span class="sxs-lookup"><span data-stu-id="6dda9-140">Simpler examples (without parameter sweeps) are presented in the main topic for regression tasks.</span></span> <span data-ttu-id="6dda9-141">Ale v příloze, jsou také uvedené ověření pomocí elastické net pro lineární regrese a odchylka nákladů pomocí parametru oblouku pro regresní náhodných doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="6dda9-141">But in the appendix, validation using elastic net for linear regression and CV with parameter sweep using for random forest regression are also presented.</span></span> <span data-ttu-id="6dda9-142">**Elastická net** je vyřešeno regrese metoda pro to lineárně hodí lineární regrese modely kombinuje metriky L1 a L2 jako postihy z [laso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) a [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) metody.</span><span class="sxs-lookup"><span data-stu-id="6dda9-142">The **elastic net** is a regularized regression method for fitting linear regression models that linearly combines the L1 and L2 metrics as penalties of the [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) and [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) methods.</span></span>   

> [!NOTE]
> <span data-ttu-id="6dda9-143">I když toolkit Spark MLlib je navržen pro práci na velkých datových sad, relativně malé ukázkové (pomocí 170 tisíc řádků, přibližně 0,1 % původní datové sady NYC ~ 30 Mb) se zde používá ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="6dda9-143">Although the Spark MLlib toolkit is designed to work on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of the original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="6dda9-144">Cvičení zadané tady běží efektivně (v přibližně 10 minut) v clusteru HDInsight s 2 uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-144">The exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="6dda9-145">Stejný kód, s menšími změnami, můžete použít ke zpracování větší-sady dat, se změny, které pro ukládání do mezipaměti data v paměti a změna velikosti clusteru.</span><span class="sxs-lookup"><span data-stu-id="6dda9-145">The same code, with minor modifications, can be used to process larger data-sets, with appropriate modifications for caching data in memory and changing the cluster size.</span></span>
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a><span data-ttu-id="6dda9-146">Instalační program: Clustery Spark a poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="6dda9-146">Setup: Spark clusters and notebooks</span></span>
<span data-ttu-id="6dda9-147">Kroky instalace a kódu jsou uvedené v tomto názorném postupu pro používání HDInsight Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="6dda9-147">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="6dda9-148">Ale poznámkové bloky Jupyter jsou k dispozici pro clustery HDInsight Spark 1.6 a Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="6dda9-148">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="6dda9-149">Popis poznámkových bloků a odkazy na ně jsou součástí [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) úložiště Githubu, které je obsahují.</span><span class="sxs-lookup"><span data-stu-id="6dda9-149">A description of the notebooks and links to them are provided in the [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for the GitHub repository containing them.</span></span> <span data-ttu-id="6dda9-150">Kromě toho kód sem a v propojených poznámkových bloků je obecný a by měla fungovat v jakémkoliv clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="6dda9-150">Moreover, the code here and in the linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="6dda9-151">Pokud nepoužíváte HDInsight Spark, může být mírně lišit od co je tady uvedené kroky nastavení a Správa clusteru.</span><span class="sxs-lookup"><span data-stu-id="6dda9-151">If you are not using HDInsight Spark, the cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="6dda9-152">Pro větší pohodlí si zde jsou uvedeny odkazy na poznámkové bloky Jupyter pro Spark 1.6 a 2.0 ke spuštění v jádra pyspark Poznámkový blok Jupyter serveru:</span><span class="sxs-lookup"><span data-stu-id="6dda9-152">For convenience, here are the links to the Jupyter notebooks for Spark 1.6 and 2.0 to be run in the pyspark kernel of the Jupyter Notebook server:</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="6dda9-153">Spark 1.6 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="6dda9-153">Spark 1.6 notebooks</span></span>

<span data-ttu-id="6dda9-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): obsahuje témata v poznámkovém bloku #1 a modelu vývoj pomocí hyperparameter ladění a křížové ověření.</span><span class="sxs-lookup"><span data-stu-id="6dda9-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Includes topics in notebook #1, and model development using hyperparameter tuning and cross-validation.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="6dda9-155">Spark 2.0 poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="6dda9-155">Spark 2.0 notebooks</span></span>

<span data-ttu-id="6dda9-156">[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Tento soubor obsahuje informace o tom, jak provést zkoumání dat, modelování a vyhodnocování clustery Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="6dda9-156">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how to perform data exploration, modeling, and scoring in Spark 2.0 clusters.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a><span data-ttu-id="6dda9-157">Instalace: umístění úložiště, knihovny a kontext přednastavené Spark</span><span class="sxs-lookup"><span data-stu-id="6dda9-157">Setup: storage locations, libraries, and the preset Spark context</span></span>
<span data-ttu-id="6dda9-158">Spark se bude moct číst a zapisovat do Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="6dda9-158">Spark is able to read and write to Azure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="6dda9-159">Takže existující data uložená existuje může zpracovat pomocí Spark a výsledky uložené v WASB znovu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-159">So any of your existing data stored there can be processed using Spark and the results stored again in WASB.</span></span>

<span data-ttu-id="6dda9-160">Cesta k uložení modely nebo souborů v WASB, je třeba zadat správně.</span><span class="sxs-lookup"><span data-stu-id="6dda9-160">To save models or files in WASB, the path needs to be specified properly.</span></span> <span data-ttu-id="6dda9-161">Výchozí kontejner, který je připojen ke clusteru Spark se může odkazovat pomocí cesty od verze: "wasb: / / /".</span><span class="sxs-lookup"><span data-stu-id="6dda9-161">The default container attached to the Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="6dda9-162">Odkazují jiné umístění "wasb: / /".</span><span class="sxs-lookup"><span data-stu-id="6dda9-162">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="6dda9-163">Nastavení cesty adresáře pro umístění úložiště v WASB</span><span class="sxs-lookup"><span data-stu-id="6dda9-163">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="6dda9-164">Následující příklad kódu určuje umístění dat ke čtení a cesty k adresáři modelu úložiště, kde je uložen výstupní modelu:</span><span class="sxs-lookup"><span data-stu-id="6dda9-164">The following code sample specifies the location of the data to be read and the path for the model storage directory to which the model output is saved:</span></span>

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="6dda9-165">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-165">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-166">DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)</span><span class="sxs-lookup"><span data-stu-id="6dda9-166">datetime.datetime(2016, 4, 18, 17, 36, 27, 832799)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="6dda9-167">Importovat knihovny</span><span class="sxs-lookup"><span data-stu-id="6dda9-167">Import libraries</span></span>
<span data-ttu-id="6dda9-168">Importujte knihovny potřebné následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6dda9-168">Import necessary libraries with the following code:</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="6dda9-169">Předvolby kontextu Spark a Magic PySpark</span><span class="sxs-lookup"><span data-stu-id="6dda9-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="6dda9-170">Jádra PySpark, které jsou k dispozici s poznámkovými bloky Jupyter mít přednastavené kontextu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-170">The PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="6dda9-171">Proto není potřeba nastavit Spark nebo vývoji Hive kontexty explicitně před zahájením práce s aplikací.</span><span class="sxs-lookup"><span data-stu-id="6dda9-171">So you do not need to set the Spark or Hive contexts explicitly before you start working with the application you are developing.</span></span> <span data-ttu-id="6dda9-172">Tyto kontexty jsou dostupné ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6dda9-172">These contexts are available for you by default.</span></span> <span data-ttu-id="6dda9-173">Tyto kontexty jsou:</span><span class="sxs-lookup"><span data-stu-id="6dda9-173">These contexts are:</span></span>

* <span data-ttu-id="6dda9-174">sc - pro Spark</span><span class="sxs-lookup"><span data-stu-id="6dda9-174">sc - for Spark</span></span> 
* <span data-ttu-id="6dda9-175">sqlContext - pro Hive</span><span class="sxs-lookup"><span data-stu-id="6dda9-175">sqlContext - for Hive</span></span>

<span data-ttu-id="6dda9-176">Poskytuje jádra PySpark některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s %%.</span><span class="sxs-lookup"><span data-stu-id="6dda9-176">The PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="6dda9-177">Existují dva takové příkazy, které se používají v tyto ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="6dda9-178">**%% místní** Určuje, že kód v dalších řádcích se má provádět místně.</span><span class="sxs-lookup"><span data-stu-id="6dda9-178">**%%local** Specifies that the code in subsequent lines is to be executed locally.</span></span> <span data-ttu-id="6dda9-179">Kód musí být platný kód Python.</span><span class="sxs-lookup"><span data-stu-id="6dda9-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="6dda9-180">**%% sql -o <variable name>**  provede dotaz Hive proti sqlContext.</span><span class="sxs-lookup"><span data-stu-id="6dda9-180">**%%sql -o <variable name>** Executes a Hive query against the sqlContext.</span></span> <span data-ttu-id="6dda9-181">Pokud je předán parametr -o, výsledek dotazu je uchován v %% lokální kontext Python jako Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="6dda9-181">If the -o parameter is passed, the result of the query is persisted in the %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="6dda9-182">Pro další informace o jádrech pro poznámkové bloky Jupyter a předdefinovanou "magics", poskytují, najdete v části [jádra dostupná pro poznámkové bloky Jupyter s HDInsight Spark Linux clusterů v HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="6dda9-182">For more information on the kernels for Jupyter notebooks and the predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="6dda9-183">Přijímání dat z veřejného objektu blob:</span><span class="sxs-lookup"><span data-stu-id="6dda9-183">Data ingestion from public blob:</span></span>
<span data-ttu-id="6dda9-184">Prvním krokem v procesu vědecké účely dat je ingestují data, která má být analyzován ze zdrojů, kde se nachází na zkoumání dat a modelování prostředí.</span><span class="sxs-lookup"><span data-stu-id="6dda9-184">The first step in the data science process is to ingest the data to be analyzed from sources where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="6dda9-185">Toto prostředí je Spark v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-185">This environment is Spark in this walkthrough.</span></span> <span data-ttu-id="6dda9-186">Tato část obsahuje kód pro dokončení řadu úloh:</span><span class="sxs-lookup"><span data-stu-id="6dda9-186">This section contains the code to complete a series of tasks:</span></span>

* <span data-ttu-id="6dda9-187">ingestování vzorek dat modelovat</span><span class="sxs-lookup"><span data-stu-id="6dda9-187">ingest the data sample to be modeled</span></span>
* <span data-ttu-id="6dda9-188">čtení ve vstupní datové sady (uložený jako soubor TSV)</span><span class="sxs-lookup"><span data-stu-id="6dda9-188">read in the input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="6dda9-189">formátování a vyčištění dat.</span><span class="sxs-lookup"><span data-stu-id="6dda9-189">format and clean the data</span></span>
* <span data-ttu-id="6dda9-190">vytvářet a ukládat do mezipaměti objektů (RDDs nebo datových rámců) v paměti</span><span class="sxs-lookup"><span data-stu-id="6dda9-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="6dda9-191">Zaregistrujte se jako dočasné tabulky v kontextu SQL.</span><span class="sxs-lookup"><span data-stu-id="6dda9-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="6dda9-192">Zde je kód pro přijímat data.</span><span class="sxs-lookup"><span data-stu-id="6dda9-192">Here is the code for data ingestion.</span></span>

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

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="6dda9-193">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-193">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-194">Doba k provedení výše buňky: 276.62 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-194">Time taken to execute above cell: 276.62 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="6dda9-195">Zkoumání dat a vizualizaci</span><span class="sxs-lookup"><span data-stu-id="6dda9-195">Data exploration & visualization</span></span>
<span data-ttu-id="6dda9-196">Jakmile data vstoupila v Spark, je dalším krokem v procesu vědecké účely dat získali lepší představu o dat přes zkoumání a vizualizace.</span><span class="sxs-lookup"><span data-stu-id="6dda9-196">Once the data has been brought into Spark, the next step in the data science process is to gain deeper understanding of the data through exploration and visualization.</span></span> <span data-ttu-id="6dda9-197">V této části jsme zkontrolujte taxíkem dat pomocí dotazů SQL a vykreslení cílových proměnných a potenciální funkcí pro visual kontroly.</span><span class="sxs-lookup"><span data-stu-id="6dda9-197">In this section, we examine the taxi data using SQL queries and plot the target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="6dda9-198">Konkrétně jsme vykreslení četnost počty osobní v taxi cest, frekvenci tip objemu a jak se typy liší podle částka platby a typu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-198">Specifically, we plot the frequency of passenger counts in taxi trips, the frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a><span data-ttu-id="6dda9-199">Vykreslení histogram frekvencí počet osobní v ukázce taxíkem cest</span><span class="sxs-lookup"><span data-stu-id="6dda9-199">Plot a histogram of passenger count frequencies in the sample of taxi trips</span></span>
<span data-ttu-id="6dda9-200">Tento kód a následné fragmenty použijte k dotazování na ukázkové a místní magic k vykreslení dat SQL magic.</span><span class="sxs-lookup"><span data-stu-id="6dda9-200">This code and subsequent snippets use SQL magic to query the sample and local magic to plot the data.</span></span>

* <span data-ttu-id="6dda9-201">**SQL magic (`%%sql`)** jádra PySpark HDInsight podporuje snadno vložené HiveQL dotazy proti sqlContext.</span><span class="sxs-lookup"><span data-stu-id="6dda9-201">**SQL magic (`%%sql`)** The HDInsight PySpark kernel supports easy inline HiveQL queries against the sqlContext.</span></span> <span data-ttu-id="6dda9-202">(-O VARIABLE_NAME) argument potrvají výstup příkazu jazyka SQL jako Pandas DataFrame na serveru Jupyter.</span><span class="sxs-lookup"><span data-stu-id="6dda9-202">The (-o VARIABLE_NAME) argument persists the output of the SQL query as a Pandas DataFrame on the Jupyter server.</span></span> <span data-ttu-id="6dda9-203">To znamená, že je k dispozici v místním režimu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-203">This means it is available in the local mode.</span></span>
* <span data-ttu-id="6dda9-204"> **`%%local` Magic** slouží ke spouštění kódu místně na serveru Jupyter, což je headnode clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6dda9-204">The **`%%local` magic** is used to run code locally on the Jupyter server, which is the headnode of the HDInsight cluster.</span></span> <span data-ttu-id="6dda9-205">Obvykle použijete, `%%local` magic po `%%sql -o` magic slouží ke spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-205">Typically, you use `%%local` magic after the `%%sql -o` magic is used to run a query.</span></span> <span data-ttu-id="6dda9-206">Parametr -o by zachovat výstup příkazu jazyka SQL místně.</span><span class="sxs-lookup"><span data-stu-id="6dda9-206">The -o parameter would persist the output of the SQL query locally.</span></span> <span data-ttu-id="6dda9-207">Pak se `%%local` magic aktivuje další sadu fragmenty kódu ke spouštění místně na výstupu dotazů SQL, který obsahuje místně trvalé.</span><span class="sxs-lookup"><span data-stu-id="6dda9-207">Then the `%%local` magic triggers the next set of code snippets to run locally against the output of the SQL queries that has been persisted locally.</span></span> <span data-ttu-id="6dda9-208">Výstup se automaticky vizualizuje po spuštění kódu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-208">The output is automatically visualized after you run the code.</span></span>

<span data-ttu-id="6dda9-209">Tento dotaz načte služebních cest podle počtu osobní.</span><span class="sxs-lookup"><span data-stu-id="6dda9-209">This query retrieves the trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


<span data-ttu-id="6dda9-210">Tento kód vytvoří místní data snímku z výstupu dotazu a ukazuje zeměpisný data.</span><span class="sxs-lookup"><span data-stu-id="6dda9-210">This code creates a local data-frame from the query output and plots the data.</span></span> <span data-ttu-id="6dda9-211">`%%local` Magic vytvoří místní rámce dat, `sqlResults`, který může být použit pro vykreslení s matplotlib.</span><span class="sxs-lookup"><span data-stu-id="6dda9-211">The `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="6dda9-212">Tato PySpark magic se používá více než jednou. v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-212">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="6dda9-213">Pokud je velké množství dat, by měl ukázkové k vytvoření data rámce, který můžete začlenit do místní paměti.</span><span class="sxs-lookup"><span data-stu-id="6dda9-213">If the amount of data is large, you should sample to create a data-frame that can fit in local memory.</span></span>
> 
> 

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="6dda9-214">Zde je kód k vykreslení služebních cest dle počtů osobní</span><span class="sxs-lookup"><span data-stu-id="6dda9-214">Here is the code to plot the trips by passenger counts</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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

<span data-ttu-id="6dda9-215">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-215">**OUTPUT**</span></span>

![Frekvence služebních cest podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

<span data-ttu-id="6dda9-217">Můžete vybrat mezi několika různých typů vizualizace (tabulky, kruhový, řádku, oblasti nebo panelu) pomocí **typ** tlačítka nabídky v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="6dda9-217">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using the **Type** menu buttons in the notebook.</span></span> <span data-ttu-id="6dda9-218">Vykreslení panelu se zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="6dda9-218">The Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="6dda9-219">Vykreslení histogram tip objemy a jak se liší podle počtu a tarif objemy osobní tip velikost.</span><span class="sxs-lookup"><span data-stu-id="6dda9-219">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="6dda9-220">Pomocí příkazu jazyka SQL ukázková data...</span><span class="sxs-lookup"><span data-stu-id="6dda9-220">Use a SQL query to sample data..</span></span>

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


<span data-ttu-id="6dda9-221">Tuto buňku kód používá k vytvoření tři pozemků data dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="6dda9-221">This code cell uses the SQL query to create three plots the data.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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


<span data-ttu-id="6dda9-222">**VÝSTUP:**</span><span class="sxs-lookup"><span data-stu-id="6dda9-222">**OUTPUT:**</span></span> 

![Tip rozdělení částky](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tip velikost podle počtu osobní](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip velikost podle tarif velikost](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="6dda9-226">Funkce analýzy, transformaci a data přípravy pro modelování</span><span class="sxs-lookup"><span data-stu-id="6dda9-226">Feature engineering, transformation, and data preparation for modeling</span></span>
<span data-ttu-id="6dda9-227">Tato část popisuje a poskytuje kód pro postupy, které slouží k přípravě dat pro použití v ML modelování.</span><span class="sxs-lookup"><span data-stu-id="6dda9-227">This section describes and provides the code for procedures used to prepare data for use in ML modeling.</span></span> <span data-ttu-id="6dda9-228">Ukazuje, jak provést následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="6dda9-228">It shows how to do the following tasks:</span></span>

* <span data-ttu-id="6dda9-229">Vytvořte novou funkci tak, že dělení čas do přihrádek doba provozu</span><span class="sxs-lookup"><span data-stu-id="6dda9-229">Create a new feature by partitioning hours into traffic time bins</span></span>
* <span data-ttu-id="6dda9-230">Index a na horkou kódování kategorií funkce</span><span class="sxs-lookup"><span data-stu-id="6dda9-230">Index and on-hot encode categorical features</span></span>
* <span data-ttu-id="6dda9-231">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="6dda9-231">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="6dda9-232">Vytvořit náhodné dílčí vzorkování dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="6dda9-232">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
* <span data-ttu-id="6dda9-233">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="6dda9-233">Feature scaling</span></span>
* <span data-ttu-id="6dda9-234">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="6dda9-234">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a><span data-ttu-id="6dda9-235">Vytvořte novou funkci tak, že dělení časy provoz do přihrádek</span><span class="sxs-lookup"><span data-stu-id="6dda9-235">Create a new feature by partitioning traffic times into bins</span></span>
<span data-ttu-id="6dda9-236">Tento kód ukazuje postup vytvořte novou funkci tak, že dělení časy provoz do přihrádek a výsledné datové rámce v paměti do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="6dda9-236">This code shows how to create a new feature by partitioning traffic times into bins and then how to cache the resulting data frame in memory.</span></span> <span data-ttu-id="6dda9-237">Ukládání do mezipaměti vede k lepší doba provádění odolné distribuované datové sady (RDDs) a datových rámců použití opakovaně.</span><span class="sxs-lookup"><span data-stu-id="6dda9-237">Caching leads to improved execution time where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly.</span></span> <span data-ttu-id="6dda9-238">Ano jsme mezipaměti RDDs a datové rámce v několika fázích v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-238">So, we cache RDDs and data-frames at several stages in this walkthrough.</span></span>

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

<span data-ttu-id="6dda9-239">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-239">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-240">126050</span><span class="sxs-lookup"><span data-stu-id="6dda9-240">126050</span></span>

### <a name="index-and-one-hot-encode-categorical-features"></a><span data-ttu-id="6dda9-241">Index a jeden horkou kódování kategorií funkce</span><span class="sxs-lookup"><span data-stu-id="6dda9-241">Index and one-hot encode categorical features</span></span>
<span data-ttu-id="6dda9-242">V této části ukazuje, jak index nebo kódování kategorií funkce pro vstup do funkce modelování.</span><span class="sxs-lookup"><span data-stu-id="6dda9-242">This section shows how to index or encode categorical features for input into the modeling functions.</span></span> <span data-ttu-id="6dda9-243">Modelování a předvídání funkce MLlib požadovat, aby se funkce s kategorií vstupní data indexované nebo kódovaný před použití.</span><span class="sxs-lookup"><span data-stu-id="6dda9-243">The modeling and predict functions of MLlib require that features with categorical input data be indexed or encoded prior to use.</span></span> 

<span data-ttu-id="6dda9-244">V závislosti na modelu budete muset index nebo zakódovat je různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="6dda9-244">Depending on the model, you need to index or encode them in different ways.</span></span> <span data-ttu-id="6dda9-245">Například Logistic a lineární regrese modely vyžadují jeden horkou kódování, kde, například funkce s tří kategorií lze rozšířit na tři sloupce funkce, s každou obsahující 0 nebo 1 v závislosti na kategorii pozorování.</span><span class="sxs-lookup"><span data-stu-id="6dda9-245">For example, Logistic and Linear Regression models require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on the category of an observation.</span></span> <span data-ttu-id="6dda9-246">Poskytuje MLlib [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkce udělat za provozu jeden kódování.</span><span class="sxs-lookup"><span data-stu-id="6dda9-246">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function to do one-hot encoding.</span></span> <span data-ttu-id="6dda9-247">Tato kodér mapuje sloupec popisek indexů ke sloupci binárního vektory, s maximálně jednu jeden – hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-247">This encoder maps a column of label indices to a column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="6dda9-248">Toto kódování umožňuje algoritmy, které očekávají číselných hodnot funkce, jako je logistic regression, má být použita pro kategorií funkce.</span><span class="sxs-lookup"><span data-stu-id="6dda9-248">This encoding allows algorithms that expect numerical valued features, such as logistic regression, to be applied to categorical features.</span></span>

<span data-ttu-id="6dda9-249">Tady je kód pro index a kódování kategorií funkce:</span><span class="sxs-lookup"><span data-stu-id="6dda9-249">Here is the code to index and encode categorical features:</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

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


<span data-ttu-id="6dda9-250">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-250">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-251">Doba k provedení nad buňku: 3.14 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-251">Time taken to execute above cell: 3.14 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="6dda9-252">Vytváření objektů s popiskem bod pro vstup do funkce ML</span><span class="sxs-lookup"><span data-stu-id="6dda9-252">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="6dda9-253">Tato část obsahuje kód, který ukazuje postup indexu kategorií textová data jako datový typ s popiskem bodu a jak zakódovat je.</span><span class="sxs-lookup"><span data-stu-id="6dda9-253">This section contains code that shows how to index categorical text data as a labeled point data type and how to encode it.</span></span> <span data-ttu-id="6dda9-254">Připraví ji mohli použít pro trénování a testování MLlib logistic regression a jinými modely klasifikace.</span><span class="sxs-lookup"><span data-stu-id="6dda9-254">This prepares it to be used to train and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="6dda9-255">S popiskem bodu objekty jsou odolné distribuované datové sady (RDD) formátu způsobem, který je nutný jako vstupní data pro většinu ML algoritmy v MLlib.</span><span class="sxs-lookup"><span data-stu-id="6dda9-255">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="6dda9-256">A [s názvem bez přípony bodu](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je přidružen místní vektoru hustých nebo zhuštění, popisek nebo odpověď.</span><span class="sxs-lookup"><span data-stu-id="6dda9-256">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="6dda9-257">Tady je kód pro index a kódování textu funkce pro binární klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="6dda9-257">Here is the code to index and encode text features for binary classification.</span></span>

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


<span data-ttu-id="6dda9-258">Tady je kód ke kódování a indexu kategorií text funkce pro analýzu lineární regrese.</span><span class="sxs-lookup"><span data-stu-id="6dda9-258">Here is the code to encode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="6dda9-259">Vytvořit náhodné dílčí vzorkování dat a rozdělit ho na trénování a testování sad</span><span class="sxs-lookup"><span data-stu-id="6dda9-259">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
<span data-ttu-id="6dda9-260">Tento kód vytvoří náhodné vzorky dat (25 % tady slouží).</span><span class="sxs-lookup"><span data-stu-id="6dda9-260">This code creates a random sampling of the data (25% is used here).</span></span> <span data-ttu-id="6dda9-261">I když to není nutné v tomto příkladu kvůli překročení velikosti datové sady, ukážeme, jak můžete zkusit tady data.</span><span class="sxs-lookup"><span data-stu-id="6dda9-261">Although it is not required for this example due to the size of the dataset, we demonstrate how you can sample the data here.</span></span> <span data-ttu-id="6dda9-262">Pak víte, jak používat pro váš vlastní problém v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6dda9-262">Then you know how to use it for your own problem if needed.</span></span> <span data-ttu-id="6dda9-263">Po velká vzorky lze ušetřit čas významné při školení modelů.</span><span class="sxs-lookup"><span data-stu-id="6dda9-263">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="6dda9-264">Další jsme rozdělit vzorku na školení část (v tomto poli 75 %) a testování částí (zde 25 %) pro použití v klasifikaci a regresní modelování.</span><span class="sxs-lookup"><span data-stu-id="6dda9-264">Next we split the sample into a training part (75% here) and a testing part (25% here) to use in classification and regression modeling.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="6dda9-265">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-265">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-266">Doba k provedení výše buňky: 0.31 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-266">Time taken to execute above cell: 0.31 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="6dda9-267">Funkce škálování</span><span class="sxs-lookup"><span data-stu-id="6dda9-267">Feature scaling</span></span>
<span data-ttu-id="6dda9-268">Funkce škálování, známé taky jako data normalizaci zajistí, že funkce široce Celková uhrazená hodnotami není zadaný nadměrné naváží ve funkci cíle.</span><span class="sxs-lookup"><span data-stu-id="6dda9-268">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in the objective function.</span></span> <span data-ttu-id="6dda9-269">Kód pro funkci škálování používá [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) funkce, které chcete odchylku jednotky škálování.</span><span class="sxs-lookup"><span data-stu-id="6dda9-269">The code for feature scaling uses the [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) to scale the features to unit variance.</span></span> <span data-ttu-id="6dda9-270">Pochází od MLlib pro použití v lineární regrese s Stochastického přechodu klesání (SGD).</span><span class="sxs-lookup"><span data-stu-id="6dda9-270">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD).</span></span> <span data-ttu-id="6dda9-271">SGD je Oblíbené algoritmus pro trénování širokou škálu jiných modelů strojového učení například Vyřešeno regresí nebo support vector počítače (SVM).</span><span class="sxs-lookup"><span data-stu-id="6dda9-271">SGD is a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>   

> [!TIP]
> <span data-ttu-id="6dda9-272">Našli jsme algoritmus LinearRegressionWithSGD být citlivé funkce škálování.</span><span class="sxs-lookup"><span data-stu-id="6dda9-272">We have found the LinearRegressionWithSGD algorithm to be sensitive to feature scaling.</span></span>   
> 
> 

<span data-ttu-id="6dda9-273">Tady je kód, který škálování proměnné pro použití s regularized lineární SGD algoritmus.</span><span class="sxs-lookup"><span data-stu-id="6dda9-273">Here is the code to scale variables for use with the regularized linear SGD algorithm.</span></span>

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

<span data-ttu-id="6dda9-274">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-274">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-275">Doba k provedení výše buňky: 11.67 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-275">Time taken to execute above cell: 11.67 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="6dda9-276">Objekty mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="6dda9-276">Cache objects in memory</span></span>
<span data-ttu-id="6dda9-277">Ukládání do mezipaměti na vstupní data rámce objekty používá pro klasifikaci, regresi a škálovat funkce může snížit čas potřebný pro trénování a testování ML algoritmů.</span><span class="sxs-lookup"><span data-stu-id="6dda9-277">The time taken for training and testing of ML algorithms can be reduced by caching the input data frame objects used for classification, regression and, scaled features.</span></span>

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

<span data-ttu-id="6dda9-278">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-278">**OUTPUT**</span></span> 

<span data-ttu-id="6dda9-279">Doba k provedení nad buňku: 0,13 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-279">Time taken to execute above cell: 0.13 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="6dda9-280">Předpovědět, zda je tip placené s modely binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="6dda9-280">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="6dda9-281">Tato část uvádí, jak použít tři modely pro predikci úlohy binární klasifikace zda tip je placené taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="6dda9-281">This section shows how use three models for the binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="6dda9-282">Modely uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="6dda9-282">The models presented are:</span></span>

* <span data-ttu-id="6dda9-283">Logistic regression</span><span class="sxs-lookup"><span data-stu-id="6dda9-283">Logistic regression</span></span> 
* <span data-ttu-id="6dda9-284">Náhodné doménové struktury</span><span class="sxs-lookup"><span data-stu-id="6dda9-284">Random forest</span></span>
* <span data-ttu-id="6dda9-285">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="6dda9-285">Gradient Boosting Trees</span></span>

<span data-ttu-id="6dda9-286">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="6dda9-286">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="6dda9-287">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="6dda9-287">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="6dda9-288">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="6dda9-288">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="6dda9-289">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="6dda9-289">**Saving model** in blob for future consumption</span></span>

<span data-ttu-id="6dda9-290">Ukážeme, jak to provést křížové ověření (odchylka nákladů) s parametrem komínů dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="6dda9-290">We show how to do cross-validation (CV) with parameter sweeping in two ways:</span></span>

1. <span data-ttu-id="6dda9-291">Pomocí **Obecné** vlastní kód, který lze použít s libovolným algoritmem v MLlib a libovolný parametr nastaví v algoritmu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-291">Using **generic** custom code which can be applied to any algorithm in MLlib and to any parameter sets in an algorithm.</span></span> 
2. <span data-ttu-id="6dda9-292">Pomocí **pySpark CrossValidator kanálu funkce**.</span><span class="sxs-lookup"><span data-stu-id="6dda9-292">Using the **pySpark CrossValidator pipeline function**.</span></span> <span data-ttu-id="6dda9-293">Všimněte si, že CrossValidator má několik omezení pro Spark 1.5.0:</span><span class="sxs-lookup"><span data-stu-id="6dda9-293">Note that CrossValidator has a few limitations for Spark 1.5.0:</span></span> 
   
   * <span data-ttu-id="6dda9-294">Modely kanálu nelze uložit nebo trvalá pro budoucí spotřeby.</span><span class="sxs-lookup"><span data-stu-id="6dda9-294">Pipeline models cannot be saved/persisted for future consumption.</span></span>
   * <span data-ttu-id="6dda9-295">Nelze použít pro každý parametr v modelu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-295">Cannot be used for every parameter in a model.</span></span>
   * <span data-ttu-id="6dda9-296">Nelze použít pro každý MLlib algoritmus.</span><span class="sxs-lookup"><span data-stu-id="6dda9-296">Cannot be used for every MLlib algorithm.</span></span>

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a><span data-ttu-id="6dda9-297">Obecná křížové ověření a hyperparameter (vymetání) komínů použít s logistic regresní algoritmus pro binární klasifikaci</span><span class="sxs-lookup"><span data-stu-id="6dda9-297">Generic cross validation and hyperparameter sweeping used with the logistic regression algorithm for binary classification</span></span>
<span data-ttu-id="6dda9-298">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-298">The code in this section shows how to train, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="6dda9-299">Cvičení modelu pomocí křížové ověření (odchylka nákladů) a hyperparameter (vymetání) komínů implementuje pomocí vlastní kód, který lze použít k některému z algoritmů učení v MLlib.</span><span class="sxs-lookup"><span data-stu-id="6dda9-299">The model is trained using cross validation (CV) and hyperparameter sweeping implemented with custom code that can be applied to any of the learning algorithms in MLlib.</span></span>   

> [!NOTE]
> <span data-ttu-id="6dda9-300">Spuštění vlastního kódu odchylka nákladů může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="6dda9-300">The execution of this custom CV code can take several minutes.</span></span>
> 
> 

<span data-ttu-id="6dda9-301">**Trénování modelu logistic regression pomocí odchylka nákladů a hyperparameter (vymetání) komínů**</span><span class="sxs-lookup"><span data-stu-id="6dda9-301">**Train the logistic regression model using CV and hyperparameter sweeping**</span></span>

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

    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
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


    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="6dda9-302">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-302">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-303">Koeficienty: [0.0082065285375-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="6dda9-303">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="6dda9-304">Zachycení:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="6dda9-304">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="6dda9-305">Doba k provedení výše buňky: 14.43 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-305">Time taken to execute above cell: 14.43 seconds</span></span>

<span data-ttu-id="6dda9-306">**Vyhodnocení modelu binární klasifikace s standardní metriky**</span><span class="sxs-lookup"><span data-stu-id="6dda9-306">**Evaluate the binary classification model with standard metrics**</span></span>

<span data-ttu-id="6dda9-307">Kód v této části ukazuje, jak vyhodnotit proti testovací data sada, včetně vykreslení křivka ROC logistic regresní model.</span><span class="sxs-lookup"><span data-stu-id="6dda9-307">The code in this section shows how to evaluate a logistic regression model against a test data-set, including a plot of the ROC curve.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="6dda9-308">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-308">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-309">Oblasti v rámci PR = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="6dda9-309">Area under PR = 0.985336538462</span></span>

<span data-ttu-id="6dda9-310">Oblasti v rámci ROC = 0.983383274312</span><span class="sxs-lookup"><span data-stu-id="6dda9-310">Area under ROC = 0.983383274312</span></span>

<span data-ttu-id="6dda9-311">Souhrnné statistiky</span><span class="sxs-lookup"><span data-stu-id="6dda9-311">Summary Stats</span></span>

<span data-ttu-id="6dda9-312">Přesnost = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="6dda9-312">Precision = 0.984174341679</span></span>

<span data-ttu-id="6dda9-313">Odvolat = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="6dda9-313">Recall = 0.984174341679</span></span>

<span data-ttu-id="6dda9-314">F1 Stanovení skóre = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="6dda9-314">F1 Score = 0.984174341679</span></span>

<span data-ttu-id="6dda9-315">Doba k provedení výše buňky: 2.67 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-315">Time taken to execute above cell: 2.67 seconds</span></span>

<span data-ttu-id="6dda9-316">**Vykreslení křivka ROC.**</span><span class="sxs-lookup"><span data-stu-id="6dda9-316">**Plot the ROC curve.**</span></span>

<span data-ttu-id="6dda9-317">*PredictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku.</span><span class="sxs-lookup"><span data-stu-id="6dda9-317">The *predictionAndLabelsDF* is registered as a table, *tmp_results*, in the previous cell.</span></span> <span data-ttu-id="6dda9-318">*tmp_results* slouží k proveďte dotazy a výstup výsledků do sqlResults dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="6dda9-318">*tmp_results* can be used to do queries and output results into the sqlResults data-frame for plotting.</span></span> <span data-ttu-id="6dda9-319">Zde je kód.</span><span class="sxs-lookup"><span data-stu-id="6dda9-319">Here is the code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="6dda9-320">Tady je kód vykreslení křivka ROC a provádět předpovědi.</span><span class="sxs-lookup"><span data-stu-id="6dda9-320">Here is the code to make predictions and plot the ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
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


<span data-ttu-id="6dda9-321">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-321">**OUTPUT**</span></span>

![Křivka ROC logistic regression pro obecný přístup](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

<span data-ttu-id="6dda9-323">**Zachovat modelu do objektu BLOB pro budoucí spotřeba**</span><span class="sxs-lookup"><span data-stu-id="6dda9-323">**Persist model in a blob for future consumption**</span></span>

<span data-ttu-id="6dda9-324">Kód v této části ukazuje, jak uložit logistic regresní model pro používání.</span><span class="sxs-lookup"><span data-stu-id="6dda9-324">The code in this section shows how to save the logistic regression model for consumption.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


<span data-ttu-id="6dda9-325">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-325">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-326">Doba k provedení výše buňky: 34.57 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-326">Time taken to execute above cell: 34.57 seconds</span></span>

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a><span data-ttu-id="6dda9-327">Pomocí funkce kanálu CrossValidator na MLlib s modelem logistic regression (elastické regrese)</span><span class="sxs-lookup"><span data-stu-id="6dda9-327">Use MLlib's CrossValidator pipeline function with logistic regression (Elastic regression) model</span></span>
<span data-ttu-id="6dda9-328">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit logistic regresní model s [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) který předpovídá, zda je tip zaplacení cesty v NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-328">The code in this section shows how to train, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="6dda9-329">Pomocí křížové ověření (odchylka nákladů) a hyperparameter (vymetání) komínů implementuje pomocí funkce MLlib CrossValidator kanálu pro odchylka nákladů pomocí parametru oblouku cvičení modelu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-329">The model is trained using cross validation (CV) and hyperparameter sweeping implemented with the MLlib CrossValidator pipeline function for CV with parameter sweep.</span></span>   

> [!NOTE]
> <span data-ttu-id="6dda9-330">Spuštění tohoto kódu odchylka MLlib nákladů může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="6dda9-330">The execution of this MLlib CV code can take several minutes.</span></span>
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

    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="6dda9-331">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-331">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-332">Doba k provedení výše buňky: 107.98 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-332">Time taken to execute above cell: 107.98 seconds</span></span>

<span data-ttu-id="6dda9-333">**Vykreslení křivka ROC.**</span><span class="sxs-lookup"><span data-stu-id="6dda9-333">**Plot the ROC curve.**</span></span>

<span data-ttu-id="6dda9-334">*PredictionAndLabelsDF* je zaregistrován jako tabulku, *tmp_results*, v předchozí buňku.</span><span class="sxs-lookup"><span data-stu-id="6dda9-334">The *predictionAndLabelsDF* is registered as a table, *tmp_results*, in the previous cell.</span></span> <span data-ttu-id="6dda9-335">*tmp_results* slouží k proveďte dotazy a výstup výsledků do sqlResults dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="6dda9-335">*tmp_results* can be used to do queries and output results into the sqlResults data-frame for plotting.</span></span> <span data-ttu-id="6dda9-336">Zde je kód.</span><span class="sxs-lookup"><span data-stu-id="6dda9-336">Here is the code.</span></span>

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

<span data-ttu-id="6dda9-337">Tady je kód k vykreslení křivka ROC.</span><span class="sxs-lookup"><span data-stu-id="6dda9-337">Here is the code to plot the ROC curve.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
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


<span data-ttu-id="6dda9-338">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-338">**OUTPUT**</span></span>

![Pomocí CrossValidator na MLlib křivka ROC logistic regression](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="6dda9-340">Klasifikace náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="6dda9-340">Random forest classification</span></span>
<span data-ttu-id="6dda9-341">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit regrese náhodných doménové struktury, který předpovídá, zda je tip zaplacení cesty v NYC taxíkem služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-341">The code in this section shows how to train, evaluate, and save a random forest regression that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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


<span data-ttu-id="6dda9-342">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-342">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-343">Oblasti v rámci ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="6dda9-343">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="6dda9-344">Doba k provedení výše buňky: 26.72 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-344">Time taken to execute above cell: 26.72 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="6dda9-345">Přechodu zvýšení skóre klasifikace stromů</span><span class="sxs-lookup"><span data-stu-id="6dda9-345">Gradient boosting trees classification</span></span>
<span data-ttu-id="6dda9-346">Kód v této části ukazuje, jak cvičení, vyhodnotit a uložte přechodu zvýšení skóre stromy model, který předpovídá, zda je pro cesty v cestě taxíkem NYC placené tip a jízdenky datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-346">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="6dda9-347">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-347">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-348">Oblasti v rámci ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="6dda9-348">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="6dda9-349">Doba k provedení výše buňky: 28.13 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-349">Time taken to execute above cell: 28.13 seconds</span></span>

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a><span data-ttu-id="6dda9-350">Předpověď tip velikost s modely regrese (nepoužíváte odchylka nákladů)</span><span class="sxs-lookup"><span data-stu-id="6dda9-350">Predict tip amount with regression models (not using CV)</span></span>
<span data-ttu-id="6dda9-351">Tato část ukazuje způsob použití, tři modely pro úlohu regrese: předpovědi tip velikost placené taxíkem cesty na základě jiných funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="6dda9-351">This section shows how use three models for the regression task: predict the tip amount paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="6dda9-352">Modely uvedené jsou:</span><span class="sxs-lookup"><span data-stu-id="6dda9-352">The models presented are:</span></span>

* <span data-ttu-id="6dda9-353">Vyřešeno lineární regrese</span><span class="sxs-lookup"><span data-stu-id="6dda9-353">Regularized linear regression</span></span>
* <span data-ttu-id="6dda9-354">Náhodné doménové struktury</span><span class="sxs-lookup"><span data-stu-id="6dda9-354">Random forest</span></span>
* <span data-ttu-id="6dda9-355">Přechodu zvýšení skóre stromů</span><span class="sxs-lookup"><span data-stu-id="6dda9-355">Gradient Boosting Trees</span></span>

<span data-ttu-id="6dda9-356">Tyto modely byly popsané v úvodu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-356">These models were described in the introduction.</span></span> <span data-ttu-id="6dda9-357">Každý model vytváření části kódu je rozdělená do kroků:</span><span class="sxs-lookup"><span data-stu-id="6dda9-357">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="6dda9-358">**Model školení** dat pomocí jednu sadu parametrů</span><span class="sxs-lookup"><span data-stu-id="6dda9-358">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="6dda9-359">**Model vyhodnocení** na testovací datové sady s metriky</span><span class="sxs-lookup"><span data-stu-id="6dda9-359">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="6dda9-360">**Ukládání modelu** v objektu blob pro budoucí spotřeba</span><span class="sxs-lookup"><span data-stu-id="6dda9-360">**Saving model** in blob for future consumption</span></span>   

> <span data-ttu-id="6dda9-361">AZURE Poznámka: Křížové ověření není použít s modely tři Regrese v této části, vzhledem k tomu, že to se zobrazí v souvislosti s modelem logistic regression.</span><span class="sxs-lookup"><span data-stu-id="6dda9-361">AZURE NOTE: Cross-validation is not used with the three regression models in this section, since this was shown in detail for the logistic regression models.</span></span> <span data-ttu-id="6dda9-362">Příkladem zobrazujícím postup používání odchylka nákladů s elastické Net pro lineární regrese najdete v příloze tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="6dda9-362">An example showing how to use CV with Elastic Net for linear regression is provided in the Appendix of this topic.</span></span>
> 
> <span data-ttu-id="6dda9-363">AZURE Poznámka: V našich zkušeností, může být problémy s konvergence LinearRegressionWithSGD modelů a parametry musí být optimalizované pečlivě pro získání platný model jejich změnit.</span><span class="sxs-lookup"><span data-stu-id="6dda9-363">AZURE NOTE: In our experience, there can be issues with convergence of LinearRegressionWithSGD models, and parameters need to be changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="6dda9-364">Škálování proměnných výrazně pomáhá s konvergence.</span><span class="sxs-lookup"><span data-stu-id="6dda9-364">Scaling of variables significantly helps with convergence.</span></span> <span data-ttu-id="6dda9-365">Elastické net regrese uveden v dodatku k tomuto tématu, můžete použít také místo LinearRegressionWithSGD.</span><span class="sxs-lookup"><span data-stu-id="6dda9-365">Elastic net regression, shown in the Appendix to this topic, can also be used instead of LinearRegressionWithSGD.</span></span>
> 
> 

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="6dda9-366">Lineární regrese s SGD</span><span class="sxs-lookup"><span data-stu-id="6dda9-366">Linear regression with SGD</span></span>
<span data-ttu-id="6dda9-367">Kód v této části ukazuje, jak používat škálovat funkce ke cvičení lineární regrese, který stochastického přechodu klesání (SGD) používá pro optimalizaci a jak stanovení skóre, hodnocení a uložit model v Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="6dda9-367">The code in this section shows how to use scaled features to train a linear regression that uses stochastic gradient descent (SGD) for optimization, and how to score, evaluate, and save the model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="6dda9-368">V našich zkušeností může být problémy s konvergence LinearRegressionWithSGD modelů a parametry musí být optimalizované pečlivě pro získání platný model jejich změnit.</span><span class="sxs-lookup"><span data-stu-id="6dda9-368">In our experience, there can be issues with the convergence of LinearRegressionWithSGD models, and parameters need to be changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="6dda9-369">Škálování proměnných výrazně pomáhá s konvergence.</span><span class="sxs-lookup"><span data-stu-id="6dda9-369">Scaling of variables significantly helps with convergence.</span></span>
> 
> 

    # LINEAR REGRESSION WITH SGD 

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="6dda9-370">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-370">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-371">Koeficienty: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]</span><span class="sxs-lookup"><span data-stu-id="6dda9-371">Coefficients: [0.0141707753435, -0.0252930927087, -0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092, -0.00456498588241, -0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632, -0.00289545676449, -0.00791124681938, 0.54396316518, -0.536293513569, 0.0119076553369, -0.0173039244582, 0.0119632796147, 0.00146764882502]</span></span>

<span data-ttu-id="6dda9-372">Zachytávat: 0.854507624459</span><span class="sxs-lookup"><span data-stu-id="6dda9-372">Intercept: 0.854507624459</span></span>

<span data-ttu-id="6dda9-373">RMSE = 1.23485131376</span><span class="sxs-lookup"><span data-stu-id="6dda9-373">RMSE = 1.23485131376</span></span>

<span data-ttu-id="6dda9-374">R sqr = 0.597963951127</span><span class="sxs-lookup"><span data-stu-id="6dda9-374">R-sqr = 0.597963951127</span></span>

<span data-ttu-id="6dda9-375">Doba k provedení výše buňky: 38.62 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-375">Time taken to execute above cell: 38.62 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="6dda9-376">Regrese náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="6dda9-376">Random Forest regression</span></span>
<span data-ttu-id="6dda9-377">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit model náhodných doménové struktury, který předpovídá velikost tip pro data NYC taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="6dda9-377">The code in this section shows how to train, evaluate, and save a random forest model that predicts tip amount for the NYC taxi trip data.</span></span>   

> [!NOTE]
> <span data-ttu-id="6dda9-378">Křížové ověření s parametrem komínů pomocí vlastního kódu najdete v příloze.</span><span class="sxs-lookup"><span data-stu-id="6dda9-378">Cross-validation with parameter sweeping using custom code is provided in the appendix.</span></span>
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
    # UN-COMMENT IF YOU WANT TO PRING TREES
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="6dda9-379">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-379">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-380">RMSE = 0.931981967875</span><span class="sxs-lookup"><span data-stu-id="6dda9-380">RMSE = 0.931981967875</span></span>

<span data-ttu-id="6dda9-381">R sqr = 0.733445485802</span><span class="sxs-lookup"><span data-stu-id="6dda9-381">R-sqr = 0.733445485802</span></span>

<span data-ttu-id="6dda9-382">Doba k provedení výše buňky: 25.98 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-382">Time taken to execute above cell: 25.98 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="6dda9-383">Přechodu zvýšení skóre regresní stromy</span><span class="sxs-lookup"><span data-stu-id="6dda9-383">Gradient boosting trees regression</span></span>
<span data-ttu-id="6dda9-384">Kód v této části ukazuje, jak pro trénování, hodnocení a uložit přechodu zvýšení skóre stromy model, který předpovídá velikost tip pro data NYC taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="6dda9-384">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts tip amount for the NYC taxi trip data.</span></span>

<span data-ttu-id="6dda9-385">** Natrénování a vyhodnocení **</span><span class="sxs-lookup"><span data-stu-id="6dda9-385">**Train and evaluate **</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="6dda9-386">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-386">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-387">RMSE = 0.928172197114</span><span class="sxs-lookup"><span data-stu-id="6dda9-387">RMSE = 0.928172197114</span></span>

<span data-ttu-id="6dda9-388">R sqr = 0.732680354389</span><span class="sxs-lookup"><span data-stu-id="6dda9-388">R-sqr = 0.732680354389</span></span>

<span data-ttu-id="6dda9-389">Doba k provedení výše buňky: 20.9 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-389">Time taken to execute above cell: 20.9 seconds</span></span>

<span data-ttu-id="6dda9-390">**Vykreslení.**</span><span class="sxs-lookup"><span data-stu-id="6dda9-390">**Plot**</span></span>

<span data-ttu-id="6dda9-391">*tmp_results* je registrován jako tabulku Hive v předchozí buňku.</span><span class="sxs-lookup"><span data-stu-id="6dda9-391">*tmp_results* is registered as a Hive table in the previous cell.</span></span> <span data-ttu-id="6dda9-392">Výsledky z tabulky se výstup do *sqlResults* dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="6dda9-392">Results from the table are output into the *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="6dda9-393">Zde je kód</span><span class="sxs-lookup"><span data-stu-id="6dda9-393">Here is the code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="6dda9-394">Tady je kód k vykreslení data s využitím serveru Jupyter.</span><span class="sxs-lookup"><span data-stu-id="6dda9-394">Here is the code to plot the data using the Jupyter server.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a><span data-ttu-id="6dda9-396">Dodatek: Další regresní úlohy křížového ověření pomocí parametru změny</span><span class="sxs-lookup"><span data-stu-id="6dda9-396">Appendix: Additional regression tasks using cross validation with parameter sweeps</span></span>
<span data-ttu-id="6dda9-397">Tento dodatek obsahuje kódu, který ukazuje, jak to provést pomocí elastické net pro lineární regrese odchylka nákladů a jak to provést odchylka nákladů s parametr oblouku pomocí vlastní kód pro regresní náhodných doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="6dda9-397">This appendix contains code showing how to do CV using Elastic net for linear regression and how to do CV with parameter sweep using custom code for random forest regression.</span></span>

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a><span data-ttu-id="6dda9-398">Křížové ověření pomocí Elastická net pro lineární regrese</span><span class="sxs-lookup"><span data-stu-id="6dda9-398">Cross validation using Elastic net for linear regression</span></span>
<span data-ttu-id="6dda9-399">Kód v této části ukazuje, jak křížové ověření pomocí elastické net pro lineární regrese a jak k vyhodnocení modelu pro testovací data.</span><span class="sxs-lookup"><span data-stu-id="6dda9-399">The code in this section shows how to do cross validation using Elastic net for linear regression and how to evaluate the model against test data.</span></span>

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
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="6dda9-400">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-400">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-401">Doba k provedení výše buňky: 161.21 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-401">Time taken to execute above cell: 161.21  seconds</span></span>

<span data-ttu-id="6dda9-402">**Vyhodnocení s metrikou R SQR**</span><span class="sxs-lookup"><span data-stu-id="6dda9-402">**Evaluate with R-SQR metric**</span></span>

<span data-ttu-id="6dda9-403">*tmp_results* je registrován jako tabulku Hive v předchozí buňku.</span><span class="sxs-lookup"><span data-stu-id="6dda9-403">*tmp_results* is registered as a Hive table in the previous cell.</span></span> <span data-ttu-id="6dda9-404">Výsledky z tabulky se výstup do *sqlResults* dat rámce pro vykreslení.</span><span class="sxs-lookup"><span data-stu-id="6dda9-404">Results from the table are output into the *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="6dda9-405">Zde je kód</span><span class="sxs-lookup"><span data-stu-id="6dda9-405">Here is the code</span></span>

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


<span data-ttu-id="6dda9-406">Tady je kód R sqr vypočítat.</span><span class="sxs-lookup"><span data-stu-id="6dda9-406">Here is the code to calculate R-sqr.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


<span data-ttu-id="6dda9-407">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-407">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-408">R sqr = 0.619184907088</span><span class="sxs-lookup"><span data-stu-id="6dda9-408">R-sqr = 0.619184907088</span></span>

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a><span data-ttu-id="6dda9-409">Křížové ověření pomocí parametru oblouku pomocí vlastní kód pro regresní náhodných doménové struktury</span><span class="sxs-lookup"><span data-stu-id="6dda9-409">Cross validation with parameter sweep using custom code for random forest regression</span></span>
<span data-ttu-id="6dda9-410">Kód v této části ukazuje, jak křížové ověření pomocí parametru oblouku pomocí vlastní kód pro regresní náhodných doménové struktury a jak k vyhodnocení modelu pro testovací data.</span><span class="sxs-lookup"><span data-stu-id="6dda9-410">The code in this section shows how to do cross validation with parameter sweep using custom code for random forest regression and how to evaluate the model against test data.</span></span>

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

    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="6dda9-411">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-411">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-412">RMSE = 0.906972198262</span><span class="sxs-lookup"><span data-stu-id="6dda9-412">RMSE = 0.906972198262</span></span>

<span data-ttu-id="6dda9-413">R sqr = 0.740751197012</span><span class="sxs-lookup"><span data-stu-id="6dda9-413">R-sqr = 0.740751197012</span></span>

<span data-ttu-id="6dda9-414">Doba k provedení výše buňky: 69.17 sekund</span><span class="sxs-lookup"><span data-stu-id="6dda9-414">Time taken to execute above cell: 69.17 seconds</span></span>

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a><span data-ttu-id="6dda9-415">Vyčištění objektů z paměti a umístění tiskové modelu</span><span class="sxs-lookup"><span data-stu-id="6dda9-415">Clean up objects from memory and print model locations</span></span>
<span data-ttu-id="6dda9-416">Použití `unpersist()` odstranit objekty uložené v mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="6dda9-416">Use `unpersist()` to delete objects cached in memory.</span></span>

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


<span data-ttu-id="6dda9-417">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-417">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-418">PythonRDD [122] v RDD v PythonRDD.scala: 43</span><span class="sxs-lookup"><span data-stu-id="6dda9-418">PythonRDD[122] at RDD at PythonRDD.scala: 43</span></span>

<span data-ttu-id="6dda9-419">** Výtisk cesta k souborům modelu, který se má použít v poznámkovém bloku spotřeby.</span><span class="sxs-lookup"><span data-stu-id="6dda9-419">**Printout path to model files to be used in the consumption notebook.</span></span> <span data-ttu-id="6dda9-420">** Pokud chcete využívat a stanovení skóre nezávislé datové sady, musíte zkopírovat a vložit tyto názvy souborů v "poznámkového bloku spotřeby".</span><span class="sxs-lookup"><span data-stu-id="6dda9-420">** To consume and score an independent data-set, you need to copy and paste these file names in the "Consumption notebook".</span></span>

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="6dda9-421">**VÝSTUP**</span><span class="sxs-lookup"><span data-stu-id="6dda9-421">**OUTPUT**</span></span>

<span data-ttu-id="6dda9-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span><span class="sxs-lookup"><span data-stu-id="6dda9-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span></span>

<span data-ttu-id="6dda9-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span><span class="sxs-lookup"><span data-stu-id="6dda9-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span></span>

<span data-ttu-id="6dda9-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span><span class="sxs-lookup"><span data-stu-id="6dda9-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span></span>

<span data-ttu-id="6dda9-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span><span class="sxs-lookup"><span data-stu-id="6dda9-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span></span>

<span data-ttu-id="6dda9-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span><span class="sxs-lookup"><span data-stu-id="6dda9-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span></span>

<span data-ttu-id="6dda9-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span><span class="sxs-lookup"><span data-stu-id="6dda9-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span></span>

## <a name="whats-next"></a><span data-ttu-id="6dda9-428">Co dále?</span><span class="sxs-lookup"><span data-stu-id="6dda9-428">What's next?</span></span>
<span data-ttu-id="6dda9-429">Teď, když jste vytvořili regrese a klasifikace modely s Spark MlLib, jste připraveni se dozvíte, jak pro stanovení skóre a vyhodnocení těchto modelů.</span><span class="sxs-lookup"><span data-stu-id="6dda9-429">Now that you have created regression and classification models with the Spark MlLib, you are ready to learn how to score and evaluate these models.</span></span>

<span data-ttu-id="6dda9-430">**Model spotřeby:** další postup stanovení skóre a vyhodnocení modelů klasifikace a regrese vytvořené v tomto tématu najdete v tématu [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="6dda9-430">**Model consumption:** To learn how to score and evaluate the classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

