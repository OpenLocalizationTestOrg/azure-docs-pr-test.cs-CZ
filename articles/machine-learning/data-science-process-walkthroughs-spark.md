---
title: "Návody pro HDInsight Spark v Azure pomocí PySpark a Scala | Microsoft Docs"
description: "Příklady procesu Team dat vědecké účely, které provede prostřednictvím PySpark a Scala v Azure HDInsight Spark pro prediktivní analýzy."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.openlocfilehash: 27065c3437c4617ed035623b48aa1a1a31a7094f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a><span data-ttu-id="43834-103">Návody vědecké účely dat HDInsight Spark v Azure pomocí PySpark a Scala</span><span class="sxs-lookup"><span data-stu-id="43834-103">HDInsight Spark data science walkthroughs using PySpark and Scala on Azure</span></span>

<span data-ttu-id="43834-104">Tyto postupy použijte PySpark a Scala v clusteru Azure Spark pro prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="43834-104">These walkthroughs use PySpark and Scala on an Azure Spark cluster to do predictive analytics.</span></span> <span data-ttu-id="43834-105">Jejich postupujte podle kroků uvedených v procesu Team dat vědecké účely.</span><span class="sxs-lookup"><span data-stu-id="43834-105">They follow the steps outlined in the Team Data Science Process.</span></span> <span data-ttu-id="43834-106">Přehled procesu Team dat vědecké účely, najdete v tématu [proces vědecké účely dat](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="43834-106">For an overview of the Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> <span data-ttu-id="43834-107">Přehled Spark v HDInsight, naleznete v části [Úvod do Spark v HDInsight](../hdinsight/hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="43834-107">For an overview of Spark on HDInsight, see [Introduction to Spark on HDInsight](../hdinsight/hdinsight-apache-spark-overview.md).</span></span>

<span data-ttu-id="43834-108">Návody vědecké účely další data, které se spouští proces vědecké účely Team dat jsou seskupené podle **platformy** , které používají:</span><span class="sxs-lookup"><span data-stu-id="43834-108">Additional data science walkthroughs that execute the Team Data Science Process are grouped by the **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a><span data-ttu-id="43834-109">Předpověď taxíkem tipy pomocí PySpark na Azure Spark</span><span class="sxs-lookup"><span data-stu-id="43834-109">Predict taxi tips using PySpark on Azure Spark</span></span>

<span data-ttu-id="43834-110">[Používejte Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md) návod používá data z New Yorku taxislužby předpovědět, zda je placené tip a rozsah objemy očekává zaplacení.</span><span class="sxs-lookup"><span data-stu-id="43834-110">The [Use Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) walkthrough uses data from New York taxis to predict whether a tip is paid and the range of amounts expected to be paid.</span></span> <span data-ttu-id="43834-111">Používá proces Team dat. vědecké účely scénář pomocí [clusteru Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/) uložit, prozkoumat a funkce pracovníka data z veřejně dostupné cestě taxíkem NYC a jízdenky datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="43834-111">It uses the Team Data Science Process in a scenario using an [Azure HDInsight Spark cluster](https://azure.microsoft.com/services/hdinsight/) to store, explore, and feature engineer data from the publicly available NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="43834-112">Toto přehledové téma sad, které si s clusteru HDInsight Spark a Jupyter PySpark poznámkových bloků použít ve zbývající části průvodce.</span><span class="sxs-lookup"><span data-stu-id="43834-112">This overview topic sets you up with an HDInsight Spark cluster and the Jupyter  PySpark notebooks used in the rest of the walkthrough.</span></span> <span data-ttu-id="43834-113">Tyto poznámkových bloků ukazují, jak prozkoumat vaše data a jak vytvářet a využívat modelů.</span><span class="sxs-lookup"><span data-stu-id="43834-113">These notebooks show you how to explore your data and then how to create and consume models.</span></span> <span data-ttu-id="43834-114">Poznámkový blok pro zkoumání a modelování pokročilé data ukazuje, jak křížového ověřování, technologie hyper parametr komínů, zahrnout a vyhodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="43834-114">The advanced data exploration and modeling notebook shows how to include cross-validation, hyper-parameter sweeping, and model evaluation.</span></span>

### <a name="data-exploration-and-modeling-with-spark"></a><span data-ttu-id="43834-115">Zkoumání dat a modelování pomocí Spark</span><span class="sxs-lookup"><span data-stu-id="43834-115">Data Exploration and modeling with Spark</span></span> 
<span data-ttu-id="43834-116">Prozkoumejte datovou sadu a vytvářet, stanovení skóre a vyhodnotit strojového učení modely projdete [vytvořit binární klasifikace a regrese modely dat pomocí Spark MLlib sady toolkit](machine-learning-data-science-spark-data-exploration-modeling.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="43834-116">Explore the dataset and create, score, and evaluate the machine learning models by working through the [Create binary classification and regression models for data with the Spark MLlib toolkit](machine-learning-data-science-spark-data-exploration-modeling.md) topic.</span></span>

### <a name="model-consumption"></a><span data-ttu-id="43834-117">Spotřeba modelu</span><span class="sxs-lookup"><span data-stu-id="43834-117">Model consumption</span></span>
<span data-ttu-id="43834-118">Informace o skóre pro klasifikaci a regrese modely, které jsou vytvořené v tomto tématu najdete v tématu [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="43834-118">To learn how to score the classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

### <a name="cross-validation-and-hyperparameter-sweeping"></a><span data-ttu-id="43834-119">Křížové ověření a hyperparameter (vymetání) komínů</span><span class="sxs-lookup"><span data-stu-id="43834-119">Cross-validation and hyperparameter sweeping</span></span>
<span data-ttu-id="43834-120">V tématu [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí sweeping křížové ověření a technologie hyper parametr.</span><span class="sxs-lookup"><span data-stu-id="43834-120">See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping.</span></span>


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a><span data-ttu-id="43834-121">Předpověď taxíkem tipy pomocí na Azure Spark Scala</span><span class="sxs-lookup"><span data-stu-id="43834-121">Predict taxi tips using Scala on Azure Spark</span></span>

<span data-ttu-id="43834-122">[Pomocí Scala pomocí Spark v Azure](machine-learning-data-science-process-scala-walkthrough.md) návod používá data z New Yorku taxislužby předpovědět, zda je placené tip a rozsah objemy očekává zaplacení.</span><span class="sxs-lookup"><span data-stu-id="43834-122">The [Use Scala with Spark on Azure](machine-learning-data-science-process-scala-walkthrough.md) walkthrough uses data from New York taxis to predict whether a tip is paid and the range of amounts expected to be paid.</span></span> <span data-ttu-id="43834-123">Ukazuje, jak používat Scala pro pod dohledem strojového učení úlohy s Spark strojového učení knihovny (MLlib) a balíčky SparkML v clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="43834-123">It shows how to use Scala for supervised machine learning tasks with the Spark machine learning library (MLlib) and SparkML packages on an Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="43834-124">Provede vás provedou úlohami, které tvoří [proces vědecké účely dat](http://aka.ms/datascienceprocess): přijímání dat a zkoumání, vizualizace, funkce analýzy, modelování a spotřeba modelu.</span><span class="sxs-lookup"><span data-stu-id="43834-124">It walks you through the tasks that constitute the [Data Science Process](http://aka.ms/datascienceprocess): data ingestion and exploration, visualization, feature engineering, modeling, and model consumption.</span></span> <span data-ttu-id="43834-125">Modely vytvářeny zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromy.</span><span class="sxs-lookup"><span data-stu-id="43834-125">The models built include logistic and linear regression, random forests, and gradient boosted trees.</span></span>


## <a name="next-steps"></a><span data-ttu-id="43834-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43834-126">Next steps</span></span>

<span data-ttu-id="43834-127">Informace součástí klíče, které tvoří proces Team dat. vědecké účely, naleznete v [přehled tým datové vědy procesu](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="43834-127">For a discussion of the key components that comprise the Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="43834-128">Informace životního cyklu Team datové vědy procesu, můžete použít pro struktury projekty vědecké účely dat, naleznete v [procesu vědecké účely Team datového cyklu](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="43834-128">For a discussion of the Team Data Science Process lifecycle that you can use to structure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="43834-129">Životní cyklus popisuje kroky, od začátku do konce, projekty obvykle postupujte při jejich spuštění.</span><span class="sxs-lookup"><span data-stu-id="43834-129">The lifecycle outlines the steps, from start to finish, that projects usually follow when they are executed.</span></span> 
