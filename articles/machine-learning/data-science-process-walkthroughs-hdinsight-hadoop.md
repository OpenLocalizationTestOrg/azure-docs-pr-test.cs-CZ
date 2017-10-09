---
title: "aaaHDInsight Hadoop vědecké účely návody k datům v Azure pomocí Hive | Microsoft Docs"
description: "Příkladem hello proces vědecké účely Team dat, které provede hello použití Hive v Azure HDInsight Hadoop toodo prediktivní analýzy."
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
ms.openlocfilehash: 77efbe4ea6377f309987849d9f44e8b2b859ae9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a><span data-ttu-id="90240-103">Používání Hive v Azure HDInsight Hadoop data vědecké účely návody</span><span class="sxs-lookup"><span data-stu-id="90240-103">HDInsight Hadoop data science walkthroughs using Hive on Azure</span></span> 

<span data-ttu-id="90240-104">Tyto postupy použijte Hive s toodo clusteru HDInsight Hadoop pro prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="90240-104">These walkthroughs use Hive with an HDInsight Hadoop cluster toodo predictive analytics.</span></span> <span data-ttu-id="90240-105">Následují hello kroků uvedených v hello proces vědecké účely dat týmu.</span><span class="sxs-lookup"><span data-stu-id="90240-105">They follow hello steps outlined in hello Team Data Science Process.</span></span> <span data-ttu-id="90240-106">Přehled hello proces vědecké účely Team dat najdete v tématu [proces vědecké účely dat](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="90240-106">For an overview of hello Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> <span data-ttu-id="90240-107">TooAzure Úvod HDInsight, naleznete v části [Úvod tooAzure HDInsight, hello technologie zásobníku Hadoop a clustery systému Hadoop](../hdinsight/hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="90240-107">For an introduction tooAzure HDInsight, see [Introduction tooAzure HDInsight, hello Hadoop technology stack, and Hadoop clusters](../hdinsight/hdinsight-hadoop-introduction.md).</span></span>

<span data-ttu-id="90240-108">Návody vědecké účely další data, které jsou spouštěny hello proces vědecké účely Team dat jsou seskupené podle hello **platformy** , které používají:</span><span class="sxs-lookup"><span data-stu-id="90240-108">Additional data science walkthroughs that execute hello Team Data Science Process are grouped by hello **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="90240-109">Předpověď taxíkem tipy pro používání Hive s HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="90240-109">Predict taxi tips using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="90240-110">Hello [clusterů systému HDInsight Hadoop pomocí](machine-learning-data-science-process-hive-walkthrough.md) návod používá data z New Yorku taxislužby toopredict:</span><span class="sxs-lookup"><span data-stu-id="90240-110">hello [Use HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) walkthrough uses data from New York taxis toopredict:</span></span> 

- <span data-ttu-id="90240-111">Jestli je placené tip</span><span class="sxs-lookup"><span data-stu-id="90240-111">Whether a tip is paid</span></span> 
- <span data-ttu-id="90240-112">distribuce Hello objemu tipu</span><span class="sxs-lookup"><span data-stu-id="90240-112">hello distribution of tip amounts</span></span>

<span data-ttu-id="90240-113">scénář Hello je implementovaná pomocí Hive se [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="90240-113">hello scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/).</span></span> <span data-ttu-id="90240-114">Zjistíte, jak toostore, prozkoumat a funkce pracovníka data z veřejně dostupné výlet taxíkem NYC a jízdenky datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="90240-114">You learn how toostore, explore, and feature engineer data from a publicly available NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="90240-115">Můžete také pomocí Azure Machine Learning toobuild a nasadit modely hello.</span><span class="sxs-lookup"><span data-stu-id="90240-115">You also use Azure Machine Learning toobuild and deploy hello models.</span></span>

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="90240-116">Předpověď klikne na oznámení o inzerovaném programu používání Hive s HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="90240-116">Predict advertisement clicks using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="90240-117">Hello [použití Azure HDInsight Hadoop clusterů v sadě dat 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) návod používá veřejně dostupné [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) zda tip je placené a hello rozsah příspěvků, klikněte na tlačítko toopredict datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="90240-117">hello [Use Azure HDInsight Hadoop Clusters on a 1-TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) walkthrough uses a publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) click dataset toopredict whether a tip is paid and hello range of amounts expected.</span></span> <span data-ttu-id="90240-118">scénář Hello je implementovaná pomocí Hive se [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) toostore, prozkoumejte, funkci pracovníka a dolů ukázková data.</span><span class="sxs-lookup"><span data-stu-id="90240-118">hello scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore, feature engineer, and down sample data.</span></span> <span data-ttu-id="90240-119">Použije toobuild Azure Machine Learning, train a score model binární klasifikace predikci, zda uživatel klikne na oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="90240-119">It uses Azure Machine Learning toobuild, train, and score a binary classification model predicting whether a user clicks on an advertisement.</span></span> <span data-ttu-id="90240-120">návod Hello se ukončí znázorňující, jak toopublish jednu z těchto modelů jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="90240-120">hello walkthrough concludes showing how toopublish one of these models as a Web service.</span></span>


## <a name="next-steps"></a><span data-ttu-id="90240-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90240-121">Next steps</span></span>

<span data-ttu-id="90240-122">Informace hello klíčových komponent, které tvoří hello proces vědecké účely dat týmu, naleznete v [přehled tým datové vědy procesu](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="90240-122">For a discussion of hello key components that comprise hello Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="90240-123">Informace životního cyklu hello proces vědecké účely Team dat, které můžete použít toostructure projekty vědecké účely dat, naleznete v [procesu vědecké účely Team datového cyklu](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="90240-123">For a discussion of hello Team Data Science Process lifecycle that you can use toostructure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="90240-124">životní cyklus Hello popisuje hello kroky, z toofinish spuštění, který projekty obvykle postupujte při jejich spuštění.</span><span class="sxs-lookup"><span data-stu-id="90240-124">hello lifecycle outlines hello steps, from start toofinish, that projects usually follow when they are executed.</span></span> 

