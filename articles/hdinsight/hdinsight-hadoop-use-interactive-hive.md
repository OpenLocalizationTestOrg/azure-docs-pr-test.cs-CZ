---
title: "Použít interaktivní Hive v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití interaktivní (Hive na LLAP) Hive v HDInsight."
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
ms.openlocfilehash: e7874b55fc72f14d8e2c801872359e823cb2ba34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a><span data-ttu-id="4d2ea-103">Použít interaktivní Hive v HDInsight (Preview)</span><span class="sxs-lookup"><span data-stu-id="4d2ea-103">Use Interactive Hive in HDInsight (Preview)</span></span>
<span data-ttu-id="4d2ea-104">Interaktivní (také známa jako Hive</span><span class="sxs-lookup"><span data-stu-id="4d2ea-104">Interactive Hive (A.K.A.</span></span> <span data-ttu-id="4d2ea-105">[Live dlouhé a proces](https://cwiki.apache.org/confluence/display/Hive/LLAP)) je nový HDInsight [clusteru typu](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="4d2ea-105">[Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is a new HDInsight [cluster type](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>  <span data-ttu-id="4d2ea-106">Umožňuje interaktivní Hive v ukládání do mezipaměti, který usnadňuje dotazů Hive mnohem víc interaktivní a rychlejší.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-106">Interactive Hive allows in memory caching that makes Hive queries much more interactive and faster.</span></span> <span data-ttu-id="4d2ea-107">Tato nová funkce umožňuje HDInsight jeden na světě většina původce, flexibilní a otevřete řešení pro velká Data do cloudu s mezipaměti v paměti (pomocí Hive a Spark) a pokročilou analýzu prostřednictvím těsná integrace s R služby.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-107">This new feature makes HDInsight one of the world’s most performant, flexible, and open Big Data solutions on the cloud with in-memory caches (using Hive and Spark) and advanced analytics through deep integration with R Services.</span></span> 

<span data-ttu-id="4d2ea-108">Interaktivní Hive clusteru se liší od clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-108">The Interactive Hive cluster is different from the Hadoop cluster.</span></span> <span data-ttu-id="4d2ea-109">Obsahuje jenom službu Hive.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-109">It only contains the Hive service.</span></span> 

> [!NOTE]
> <span data-ttu-id="4d2ea-110">MapReduce, Pig, Sqoop, Oozie a dalším službám se odeberou z tohoto typu clusteru brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-110">MapReduce, Pig, Sqoop, Oozie, and other services will be removed from this cluster type soon.</span></span>
> <span data-ttu-id="4d2ea-111">Službu Hive v clusteru interaktivní Hive přístupný jenom prostřednictvím zobrazení Ambari Hive, Beeline a Hive ODBC.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-111">The Hive service in the Interactive Hive cluster is only accessible via the Ambari Hive view, Beeline, and Hive ODBC.</span></span> <span data-ttu-id="4d2ea-112">Není přístupná prostřednictvím konzoly Hive, Templeton, rozhraní příkazového řádku Azure a prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-112">It can’t be accessed via Hive console, Templeton, Azure CLI, and Azure PowerShell.</span></span> 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a><span data-ttu-id="4d2ea-113">Vytvoření clusteru interaktivní Hive</span><span class="sxs-lookup"><span data-stu-id="4d2ea-113">Create an Interactive Hive cluster</span></span>
<span data-ttu-id="4d2ea-114">Interaktivní Hive clusteru je podporována pouze na clusterech se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-114">Interactive Hive cluster is only supported on Linux-based clusters.</span></span> <span data-ttu-id="4d2ea-115">Informace o vytváření clusterů HDInsight naleznete v tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="4d2ea-115">For information about creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="execute-hive-from-interactive-hive"></a><span data-ttu-id="4d2ea-116">Spuštění Hive z interaktivní Hive</span><span class="sxs-lookup"><span data-stu-id="4d2ea-116">Execute Hive from Interactive Hive</span></span>
<span data-ttu-id="4d2ea-117">Existují různé možnosti, jak můžete provádět dotazy Hive:</span><span class="sxs-lookup"><span data-stu-id="4d2ea-117">There are different options how you can execute Hive queries:</span></span>

* <span data-ttu-id="4d2ea-118">Spuštění Hive pomocí zobrazení Ambari Hive</span><span class="sxs-lookup"><span data-stu-id="4d2ea-118">Run Hive using the Ambari Hive view</span></span>
  
    <span data-ttu-id="4d2ea-119">Informace o používání zobrazení Hive naleznete v tématu [použít zobrazení Hive se systémem Hadoop v HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="4d2ea-119">For the information about using the Hive View, see [Use the Hive View with Hadoop in HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>
* <span data-ttu-id="4d2ea-120">Spuštění Hive pomocí Beeline</span><span class="sxs-lookup"><span data-stu-id="4d2ea-120">Run Hive using Beeline</span></span>
  
    <span data-ttu-id="4d2ea-121">Informace o používání Beeline v HDInsight naleznete v tématu [používání Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).</span><span class="sxs-lookup"><span data-stu-id="4d2ea-121">For the information on using Beeline on HDInsight, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
  
    <span data-ttu-id="4d2ea-122">Používáte Beeline z headnode nebo prázdný hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-122">You use Beeline from either the headnode or an empty edge node.</span></span>  <span data-ttu-id="4d2ea-123">Doporučuje se použít Beeline z prázdné hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-123">Using Beeline from an empty edge node is recommended.</span></span>  <span data-ttu-id="4d2ea-124">Informace o vytváření clusteru HDInsight se prázdný edgenode najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="4d2ea-124">For information on creating an HDInsight cluster with an empty edgenode, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>
* <span data-ttu-id="4d2ea-125">Spuštění Hive pomocí ovladače ODBC Hive</span><span class="sxs-lookup"><span data-stu-id="4d2ea-125">Run Hive using Hive ODBC</span></span>
  
    <span data-ttu-id="4d2ea-126">Informace o používání Hive ODBC naleznete v tématu [připojit Excel k systému Hadoop pomocí ovladače Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="4d2ea-126">For the information on using Hive ODBC, see [Connect Excel to Hadoop with the Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>

<span data-ttu-id="4d2ea-127">**Vyhledání JDBC připojovací řetězec:**</span><span class="sxs-lookup"><span data-stu-id="4d2ea-127">**To find the JDBC connection string:**</span></span>

1. <span data-ttu-id="4d2ea-128">Přihlaste se pomocí následující adresy URL Ambari: https://<ClusterName>. AzureHDInsight.net.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-128">Sign on to Ambari using the following URL: https://<ClusterName>.AzureHDInsight.net.</span></span>
2. <span data-ttu-id="4d2ea-129">Klikněte na tlačítko **Hive** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-129">Click **Hive** from the left menu.</span></span>
3. <span data-ttu-id="4d2ea-130">Klikněte na ikonu zvýrazněná zkopírujte adresu URL:</span><span class="sxs-lookup"><span data-stu-id="4d2ea-130">Click the highlighted icon to copy the URL:</span></span>
   
   ![HDInsight Hadoop Hive interaktivní LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a><span data-ttu-id="4d2ea-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="4d2ea-132">See also</span></span>
* <span data-ttu-id="4d2ea-133">[Vytvořit clustery se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Naučte se vytvářet clustery interaktivní Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-133">[Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Learn how to create Interactive Hive clusters in HDInsight.</span></span>
* <span data-ttu-id="4d2ea-134">[Použijte Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md): Naučte se používat Beeline k odesílání dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-134">[Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md): Learn how to use Beeline to submit Hive queries.</span></span>
* <span data-ttu-id="4d2ea-135">[Připojení aplikace Excel k systému Hadoop pomocí ovladače Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md): Zjistěte, jak připojit Excel k Hive.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-135">[Connect Excel to Hadoop with the Microsoft Hive ODBC driver](hdinsight-connect-excel-hive-odbc-driver.md): learn how to connect Excel to Hive.</span></span>

