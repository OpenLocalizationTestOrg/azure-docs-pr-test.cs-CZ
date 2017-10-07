---
title: "aaaUse interaktivní Hive v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse interaktivní (Hive na LLAP) Hive v HDInsight."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 9e751a08091d18bc1b3d070468feef87f6828c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a><span data-ttu-id="e86ce-103">Použít interaktivní Hive v HDInsight (Preview)</span><span class="sxs-lookup"><span data-stu-id="e86ce-103">Use Interactive Hive in HDInsight (Preview)</span></span>
<span data-ttu-id="e86ce-104">Interaktivní (také známa jako Hive</span><span class="sxs-lookup"><span data-stu-id="e86ce-104">Interactive Hive (A.K.A.</span></span> <span data-ttu-id="e86ce-105">[Live dlouhé a proces](https://cwiki.apache.org/confluence/display/Hive/LLAP)) je nový HDInsight [clusteru typu](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="e86ce-105">[Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is a new HDInsight [cluster type](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>  <span data-ttu-id="e86ce-106">Umožňuje interaktivní Hive v ukládání do mezipaměti, který usnadňuje dotazů Hive mnohem víc interaktivní a rychlejší.</span><span class="sxs-lookup"><span data-stu-id="e86ce-106">Interactive Hive allows in memory caching that makes Hive queries much more interactive and faster.</span></span> <span data-ttu-id="e86ce-107">Tato nová funkce umožňuje HDInsight jeden hello světě většina původce, flexibilní a otevřete řešení pro velká Data v cloudu hello s mezipaměti v paměti (pomocí Hive a Spark) a pokročilou analýzu prostřednictvím těsná integrace s R služby.</span><span class="sxs-lookup"><span data-stu-id="e86ce-107">This new feature makes HDInsight one of hello world’s most performant, flexible, and open Big Data solutions on hello cloud with in-memory caches (using Hive and Spark) and advanced analytics through deep integration with R Services.</span></span> 

<span data-ttu-id="e86ce-108">Hello interaktivní Hive clusteru se liší od clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="e86ce-108">hello Interactive Hive cluster is different from hello Hadoop cluster.</span></span> <span data-ttu-id="e86ce-109">Obsahuje jenom hello podregistru služby.</span><span class="sxs-lookup"><span data-stu-id="e86ce-109">It only contains hello Hive service.</span></span> 

> [!NOTE]
> <span data-ttu-id="e86ce-110">MapReduce, Pig, Sqoop, Oozie a dalším službám se odeberou z tohoto typu clusteru brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e86ce-110">MapReduce, Pig, Sqoop, Oozie, and other services will be removed from this cluster type soon.</span></span>
> <span data-ttu-id="e86ce-111">Hello služby Hive v clusteru interaktivní Hive hello přístupný jenom prostřednictvím hello zobrazení Ambari Hive, Beeline a Hive ODBC.</span><span class="sxs-lookup"><span data-stu-id="e86ce-111">hello Hive service in hello Interactive Hive cluster is only accessible via hello Ambari Hive view, Beeline, and Hive ODBC.</span></span> <span data-ttu-id="e86ce-112">Není přístupná prostřednictvím konzoly Hive, Templeton, rozhraní příkazového řádku Azure a prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e86ce-112">It can’t be accessed via Hive console, Templeton, Azure CLI, and Azure PowerShell.</span></span> 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a><span data-ttu-id="e86ce-113">Vytvoření clusteru interaktivní Hive</span><span class="sxs-lookup"><span data-stu-id="e86ce-113">Create an Interactive Hive cluster</span></span>
<span data-ttu-id="e86ce-114">Interaktivní Hive clusteru je podporována pouze na clusterech se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="e86ce-114">Interactive Hive cluster is only supported on Linux-based clusters.</span></span> <span data-ttu-id="e86ce-115">Informace o vytváření clusterů HDInsight naleznete v tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="e86ce-115">For information about creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="execute-hive-from-interactive-hive"></a><span data-ttu-id="e86ce-116">Spuštění Hive z interaktivní Hive</span><span class="sxs-lookup"><span data-stu-id="e86ce-116">Execute Hive from Interactive Hive</span></span>
<span data-ttu-id="e86ce-117">Existují různé možnosti, jak můžete provádět dotazy Hive:</span><span class="sxs-lookup"><span data-stu-id="e86ce-117">There are different options how you can execute Hive queries:</span></span>

* <span data-ttu-id="e86ce-118">Spuštění Hive pomocí hello zobrazení Ambari Hive</span><span class="sxs-lookup"><span data-stu-id="e86ce-118">Run Hive using hello Ambari Hive view</span></span>
  
    <span data-ttu-id="e86ce-119">Hello informace o používání hello zobrazení Hive naleznete v tématu [použití hello zobrazení Hive se systémem Hadoop v HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="e86ce-119">For hello information about using hello Hive View, see [Use hello Hive View with Hadoop in HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>
* <span data-ttu-id="e86ce-120">Spuštění Hive pomocí Beeline</span><span class="sxs-lookup"><span data-stu-id="e86ce-120">Run Hive using Beeline</span></span>
  
    <span data-ttu-id="e86ce-121">Hello informace o používání Beeline v HDInsight naleznete v tématu [používání Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).</span><span class="sxs-lookup"><span data-stu-id="e86ce-121">For hello information on using Beeline on HDInsight, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
  
    <span data-ttu-id="e86ce-122">Používáte Beeline z hello headnode nebo prázdný hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="e86ce-122">You use Beeline from either hello headnode or an empty edge node.</span></span>  <span data-ttu-id="e86ce-123">Doporučuje se použít Beeline z prázdné hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="e86ce-123">Using Beeline from an empty edge node is recommended.</span></span>  <span data-ttu-id="e86ce-124">Informace o vytváření clusteru HDInsight se prázdný edgenode najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="e86ce-124">For information on creating an HDInsight cluster with an empty edgenode, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>
* <span data-ttu-id="e86ce-125">Spuštění Hive pomocí ovladače ODBC Hive</span><span class="sxs-lookup"><span data-stu-id="e86ce-125">Run Hive using Hive ODBC</span></span>
  
    <span data-ttu-id="e86ce-126">Hello informace o používání Hive ODBC naleznete v tématu [tooHadoop připojení aplikace Excel pomocí ovladače Microsoft Hive ODBC hello](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="e86ce-126">For hello information on using Hive ODBC, see [Connect Excel tooHadoop with hello Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>

<span data-ttu-id="e86ce-127">**toofind hello JDBC připojovací řetězec:**</span><span class="sxs-lookup"><span data-stu-id="e86ce-127">**toofind hello JDBC connection string:**</span></span>

1. <span data-ttu-id="e86ce-128">Přihlaste se pomocí hello následující adresu URL tooAmbari: https://<ClusterName>. AzureHDInsight.net.</span><span class="sxs-lookup"><span data-stu-id="e86ce-128">Sign on tooAmbari using hello following URL: https://<ClusterName>.AzureHDInsight.net.</span></span>
2. <span data-ttu-id="e86ce-129">Klikněte na tlačítko **Hive** hello levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="e86ce-129">Click **Hive** from hello left menu.</span></span>
3. <span data-ttu-id="e86ce-130">Klikněte na tlačítko hello zvýrazněná ikonu toocopy hello URL:</span><span class="sxs-lookup"><span data-stu-id="e86ce-130">Click hello highlighted icon toocopy hello URL:</span></span>
   
   ![HDInsight Hadoop Hive interaktivní LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a><span data-ttu-id="e86ce-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="e86ce-132">See also</span></span>
* <span data-ttu-id="e86ce-133">[Vytvořit clustery se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Zjistěte, jak clusterů toocreate interaktivní Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e86ce-133">[Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Learn how toocreate Interactive Hive clusters in HDInsight.</span></span>
* <span data-ttu-id="e86ce-134">[Použijte Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md): Zjistěte, jak dotazů Hive toosubmit Beeline toouse.</span><span class="sxs-lookup"><span data-stu-id="e86ce-134">[Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md): Learn how toouse Beeline toosubmit Hive queries.</span></span>
* <span data-ttu-id="e86ce-135">[Připojení aplikace Excel tooHadoop pomocí ovladače Microsoft Hive ODBC hello](hdinsight-connect-excel-hive-odbc-driver.md): Zjistěte, jak tooHive tooconnect aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="e86ce-135">[Connect Excel tooHadoop with hello Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md): learn how tooconnect Excel tooHive.</span></span>

