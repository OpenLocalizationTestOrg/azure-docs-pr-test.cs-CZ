---
title: "Používání Hive v Azure HDInsight Hadoop data vědecké účely návody | Microsoft Docs"
description: "Příklady procesu Team dat vědecké účely, které provede pomocí Hive v Azure HDInsight Hadoop pro prediktivní analýzy."
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
ms.openlocfilehash: fb06c3c1b1ae30d970a2e4d45a49e22e9d78b876
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a><span data-ttu-id="0d985-103">Používání Hive v Azure HDInsight Hadoop data vědecké účely návody</span><span class="sxs-lookup"><span data-stu-id="0d985-103">HDInsight Hadoop data science walkthroughs using Hive on Azure</span></span> 

<span data-ttu-id="0d985-104">Tyto postupy použijte Hive s clusteru HDInsight Hadoop pro prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="0d985-104">These walkthroughs use Hive with an HDInsight Hadoop cluster to do predictive analytics.</span></span> <span data-ttu-id="0d985-105">Jejich postupujte podle kroků uvedených v procesu Team dat vědecké účely.</span><span class="sxs-lookup"><span data-stu-id="0d985-105">They follow the steps outlined in the Team Data Science Process.</span></span> <span data-ttu-id="0d985-106">Přehled procesu Team dat vědecké účely, najdete v tématu [proces vědecké účely dat](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0d985-106">For an overview of the Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> <span data-ttu-id="0d985-107">Úvod do Azure HDInsight, naleznete v části [Úvod do Azure HDInsight, technologie zásobníku Hadoop a clustery systému Hadoop](../hdinsight/hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0d985-107">For an introduction to Azure HDInsight, see [Introduction to Azure HDInsight, the Hadoop technology stack, and Hadoop clusters](../hdinsight/hdinsight-hadoop-introduction.md).</span></span>

<span data-ttu-id="0d985-108">Návody vědecké účely další data, které se spouští proces vědecké účely Team dat jsou seskupené podle **platformy** , které používají:</span><span class="sxs-lookup"><span data-stu-id="0d985-108">Additional data science walkthroughs that execute the Team Data Science Process are grouped by the **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="0d985-109">Předpověď taxíkem tipy pro používání Hive s HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="0d985-109">Predict taxi tips using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="0d985-110">[Clusterů systému HDInsight Hadoop pomocí](machine-learning-data-science-process-hive-walkthrough.md) návod používá data z New Yorku taxislužby k předvídání:</span><span class="sxs-lookup"><span data-stu-id="0d985-110">The [Use HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) walkthrough uses data from New York taxis to predict:</span></span> 

- <span data-ttu-id="0d985-111">Jestli je placené tip</span><span class="sxs-lookup"><span data-stu-id="0d985-111">Whether a tip is paid</span></span> 
- <span data-ttu-id="0d985-112">Rozdělení objemu tipu</span><span class="sxs-lookup"><span data-stu-id="0d985-112">The distribution of tip amounts</span></span>

<span data-ttu-id="0d985-113">Tento scénář je implementovaná pomocí Hive se [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="0d985-113">The scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/).</span></span> <span data-ttu-id="0d985-114">Zjistíte, jak k uložení prozkoumat, a funkce pracovníka data z veřejně dostupné NYC taxi služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="0d985-114">You learn how to store, explore, and feature engineer data from a publicly available NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="0d985-115">Je také použít k vytváření a nasazování modely Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0d985-115">You also use Azure Machine Learning to build and deploy the models.</span></span>

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="0d985-116">Předpověď klikne na oznámení o inzerovaném programu používání Hive s HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="0d985-116">Predict advertisement clicks using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="0d985-117">[Použití Azure HDInsight Hadoop clusterů v sadě dat 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) návod používá veřejně dostupné [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) klikněte na datovou sadu, která předvídat, jestli je tip placené a rozsah objemy očekává.</span><span class="sxs-lookup"><span data-stu-id="0d985-117">The [Use Azure HDInsight Hadoop Clusters on a 1-TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) walkthrough uses a publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) click dataset to predict whether a tip is paid and the range of amounts expected.</span></span> <span data-ttu-id="0d985-118">Tento scénář je implementovaná pomocí Hive se [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) k uložení, prozkoumejte, funkci pracovníka a dolů ukázková data.</span><span class="sxs-lookup"><span data-stu-id="0d985-118">The scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) to store, explore, feature engineer, and down sample data.</span></span> <span data-ttu-id="0d985-119">Azure Machine Learning používá pro vytváření, trénování a určení skóre modelu binární klasifikace predikci, zda uživatel klikne na oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="0d985-119">It uses Azure Machine Learning to build, train, and score a binary classification model predicting whether a user clicks on an advertisement.</span></span> <span data-ttu-id="0d985-120">Průvodce se ukončí znázorňující k publikování některého z těchto modelů jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="0d985-120">The walkthrough concludes showing how to publish one of these models as a Web service.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0d985-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d985-121">Next steps</span></span>

<span data-ttu-id="0d985-122">Informace součástí klíče, které tvoří proces Team dat. vědecké účely, naleznete v [přehled tým datové vědy procesu](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0d985-122">For a discussion of the key components that comprise the Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="0d985-123">Informace životního cyklu Team datové vědy procesu, můžete použít pro struktury projekty vědecké účely dat, naleznete v [procesu vědecké účely Team datového cyklu](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="0d985-123">For a discussion of the Team Data Science Process lifecycle that you can use to structure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="0d985-124">Životní cyklus popisuje kroky, od začátku do konce, projekty obvykle postupujte při jejich spuštění.</span><span class="sxs-lookup"><span data-stu-id="0d985-124">The lifecycle outlines the steps, from start to finish, that projects usually follow when they are executed.</span></span> 

