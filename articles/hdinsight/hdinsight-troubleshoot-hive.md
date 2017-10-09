---
title: "aaaTroubleshoot Hive pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi toocommon dotazy týkající se práce s Apache Hive a Azure HDInsight."
keywords: "Azure HDInsight Hive, – nejčastější dotazy, řešení potíží s průvodce, časté otázky"
services: Azure HDInsight
documentationcenter: na
author: dharmeshkakadia
manager: 
editor: 
ms.assetid: 15B8D0F3-F2D3-4746-BDCB-C72944AA9252
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: dharmeshkakadia
ms.openlocfilehash: ac459316e658d0b29eb66f5685f0bc7e693bb277
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a><span data-ttu-id="27b55-104">Řešení potíží Hive pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="27b55-104">Troubleshoot Hive by using Azure HDInsight</span></span>

<span data-ttu-id="27b55-105">Další informace o hello nejvyšší otázky a jejich řešení při práci s Apache Hive datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="27b55-105">Learn about hello top questions and their resolutions when working with Apache Hive payloads in Apache Ambari.</span></span>


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a><span data-ttu-id="27b55-106">Jak exportovat metaúložiště Hive a importovat na jiný cluster</span><span class="sxs-lookup"><span data-stu-id="27b55-106">How do I export a Hive metastore and import it on another cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="27b55-107">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="27b55-107">Resolution steps</span></span>

1. <span data-ttu-id="27b55-108">Připojte toohello clusteru HDInsight pomocí klienta Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="27b55-108">Connect toohello HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="27b55-109">Další informace najdete v tématu [další čtení](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="27b55-109">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="27b55-110">Spusťte následující příkaz na clusteru HDInsight hello, ze kterého mají být tooexport hello metaúložiště hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-110">Run hello following command on hello HDInsight cluster from which you want tooexport hello metastore:</span></span>

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  <span data-ttu-id="27b55-111">Tento příkaz vytvoří soubor s názvem allatables.sql.</span><span class="sxs-lookup"><span data-stu-id="27b55-111">This command generates a file named allatables.sql.</span></span>

3. <span data-ttu-id="27b55-112">Zkopírujte hello souboru alltables.sql toohello nový cluster HDInsight a pak spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-112">Copy hello file alltables.sql toohello new HDInsight cluster, and then run hello following command:</span></span>

  ```apache
  hive -f alltables.sql
  ```

<span data-ttu-id="27b55-113">Hello kód v kroky řešení hello předpokládá, že data, která jsou cesty na novém clusteru hello stejné hello jako hello cesty k datům v původním clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="27b55-113">hello code in hello resolution steps assumes that data paths on hello new cluster are hello same as hello data paths on hello old cluster.</span></span> <span data-ttu-id="27b55-114">Pokud cesty k datům hello liší, můžete ručně upravit hello generované alltables.sql souboru tooreflect žádné změny.</span><span class="sxs-lookup"><span data-stu-id="27b55-114">If hello data paths are different, you can manually edit hello generated alltables.sql file tooreflect any changes.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="27b55-115">Další čtení</span><span class="sxs-lookup"><span data-stu-id="27b55-115">Additional reading</span></span>

- [<span data-ttu-id="27b55-116">Připojit tooan clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="27b55-116">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a><span data-ttu-id="27b55-117">Jak najít protokoly Hive v clusteru</span><span class="sxs-lookup"><span data-stu-id="27b55-117">How do I locate Hive logs on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="27b55-118">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="27b55-118">Resolution steps</span></span>

1. <span data-ttu-id="27b55-119">Připojte toohello clusteru HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="27b55-119">Connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="27b55-120">Další informace najdete v tématu **další čtení**.</span><span class="sxs-lookup"><span data-stu-id="27b55-120">For more information, see **Additional reading**.</span></span>

2. <span data-ttu-id="27b55-121">protokoly klienta tooview Hive, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-121">tooview Hive client logs, use hello following command:</span></span>

  ```apache
  /tmp/<username>/hive.log 
  ```

3. <span data-ttu-id="27b55-122">protokoly metaúložiště Hive tooview, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-122">tooview Hive metastore logs, use hello following command:</span></span>

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. <span data-ttu-id="27b55-123">tooview Hiveserver protokoly, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-123">tooview Hiveserver logs, use hello following command:</span></span>

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a><span data-ttu-id="27b55-124">Další čtení</span><span class="sxs-lookup"><span data-stu-id="27b55-124">Additional reading</span></span>

- [<span data-ttu-id="27b55-125">Připojit tooan clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="27b55-125">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-hello-hive-shell-with-specific-configurations-on-a-cluster"></a><span data-ttu-id="27b55-126">Jak spuštění hello prostředí Hive s konkrétní konfigurací v clusteru</span><span class="sxs-lookup"><span data-stu-id="27b55-126">How do I launch hello Hive shell with specific configurations on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="27b55-127">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="27b55-127">Resolution steps</span></span>

1. <span data-ttu-id="27b55-128">Zadejte pár klíč hodnota konfigurace při spuštění hello prostředí Hive.</span><span class="sxs-lookup"><span data-stu-id="27b55-128">Specify a configuration key-value pair when you start hello Hive shell.</span></span> <span data-ttu-id="27b55-129">Další informace najdete v tématu [další čtení](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="27b55-129">For more information, see [Additional reading](#additional-reading-end).</span></span>

  ```apache
  hive -hiveconf a=b 
  ```

2. <span data-ttu-id="27b55-130">toolist všechny efektivní konfigurace v prostředí Hive, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="27b55-130">toolist all effective configurations on Hive shell, use hello following command:</span></span>

  ```apache
  hive> set;
  ```

  <span data-ttu-id="27b55-131">Například použijte následující příkaz prostředí Hive toostart s protokolováním ladění povoleno v konzole hello hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-131">For example, use hello following command toostart Hive shell with debug logging enabled on hello console:</span></span>

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a><span data-ttu-id="27b55-132">Další čtení</span><span class="sxs-lookup"><span data-stu-id="27b55-132">Additional reading</span></span>

- [<span data-ttu-id="27b55-133">Vlastnosti konfigurace Hive</span><span class="sxs-lookup"><span data-stu-id="27b55-133">Hive configuration properties</span></span>](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <span data-ttu-id="27b55-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Jak můžu analyzovat data Tez DAG v clusteru kritické cestě</span><span class="sxs-lookup"><span data-stu-id="27b55-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>How do I analyze Tez DAG data on a cluster-critical path</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="27b55-135">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="27b55-135">Resolution steps</span></span>
 
1. <span data-ttu-id="27b55-136">tooanalyze Apache Tez řízené Acyklické grafu (DAG) na clusteru kritické graf, připojit toohello clusteru HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="27b55-136">tooanalyze an Apache Tez directed acyclic graph (DAG) on a cluster-critical graph, connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="27b55-137">Další informace najdete v tématu [další čtení](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="27b55-137">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="27b55-138">Na příkazovém řádku spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-138">At a command prompt, run hello following command:</span></span>
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. <span data-ttu-id="27b55-139">toolist jiné analyzátory, které se dají použít tooanalyze Tez DAG použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="27b55-139">toolist other analyzers that can be used tooanalyze Tez DAG, use hello following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  <span data-ttu-id="27b55-140">Ukázkový program je nutné zadat jako první argument hello.</span><span class="sxs-lookup"><span data-stu-id="27b55-140">You must provide an example program as hello first argument.</span></span>

  <span data-ttu-id="27b55-141">Program platné názvy patří:</span><span class="sxs-lookup"><span data-stu-id="27b55-141">Valid program names include:</span></span>
    - <span data-ttu-id="27b55-142">**ContainerReuseAnalyzer**: tisk podrobností o opakované použití kontejneru v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-142">**ContainerReuseAnalyzer**: Print container reuse details in a DAG</span></span>
    - <span data-ttu-id="27b55-143">**CriticalPath**: Find hello kritické cesta DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-143">**CriticalPath**: Find hello critical path of a DAG</span></span>
    - <span data-ttu-id="27b55-144">**LocalityAnalyzer**: tisk polohu podrobností v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-144">**LocalityAnalyzer**: Print locality details in a DAG</span></span>
    - <span data-ttu-id="27b55-145">**ShuffleTimeAnalyzer**: analyzovat podrobnosti času náhodně hello v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-145">**ShuffleTimeAnalyzer**: Analyze hello shuffle time details in a DAG</span></span>
    - <span data-ttu-id="27b55-146">**SkewAnalyzer**: Analýza hello zkosení podrobnosti v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-146">**SkewAnalyzer**: Analyze hello skew details in a DAG</span></span>
    - <span data-ttu-id="27b55-147">**SlowNodeAnalyzer**: tisk podrobností uzlu v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-147">**SlowNodeAnalyzer**: Print node details in a DAG</span></span>
    - <span data-ttu-id="27b55-148">**SlowTaskIdentifier**: tisk podrobnosti pomalé úlohy v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-148">**SlowTaskIdentifier**: Print slow task details in a DAG</span></span>
    - <span data-ttu-id="27b55-149">**SlowestVertexAnalyzer**: tisk nejpomalejší vrchol podrobností v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-149">**SlowestVertexAnalyzer**: Print slowest vertex details in a DAG</span></span>
    - <span data-ttu-id="27b55-150">**SpillAnalyzer**: tisk přepadového podrobnosti v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-150">**SpillAnalyzer**: Print spill details in a DAG</span></span>
    - <span data-ttu-id="27b55-151">**TaskConcurrencyAnalyzer**: tisk souběžnosti podrobnosti úlohy hello v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-151">**TaskConcurrencyAnalyzer**: Print hello task concurrency details in a DAG</span></span>
    - <span data-ttu-id="27b55-152">**VertexLevelCriticalPathAnalyzer**: najít hello kritické cesty na úrovni vrchol v DAG</span><span class="sxs-lookup"><span data-stu-id="27b55-152">**VertexLevelCriticalPathAnalyzer**: Find hello critical path at vertex level in a DAG</span></span>


### <a name="additional-reading"></a><span data-ttu-id="27b55-153">Další čtení</span><span class="sxs-lookup"><span data-stu-id="27b55-153">Additional reading</span></span>

- [<span data-ttu-id="27b55-154">Připojit tooan clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="27b55-154">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a><span data-ttu-id="27b55-155">Jak lze stáhnout Tez DAG data z clusteru</span><span class="sxs-lookup"><span data-stu-id="27b55-155">How do I download Tez DAG data from a cluster</span></span>


#### <a name="resolution-steps"></a><span data-ttu-id="27b55-156">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="27b55-156">Resolution steps</span></span>

<span data-ttu-id="27b55-157">Existují dva způsoby toocollect hello Tez DAG dat:</span><span class="sxs-lookup"><span data-stu-id="27b55-157">There are two ways toocollect hello Tez DAG data:</span></span>

- <span data-ttu-id="27b55-158">Z příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-158">From hello command line:</span></span>
 
    <span data-ttu-id="27b55-159">Připojte toohello clusteru HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="27b55-159">Connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="27b55-160">Hello příkazového řádku spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="27b55-160">At hello command prompt, run hello following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- <span data-ttu-id="27b55-161">Použijte hello zobrazení Ambari Tez:</span><span class="sxs-lookup"><span data-stu-id="27b55-161">Use hello Ambari Tez view:</span></span>
   
  1. <span data-ttu-id="27b55-162">Přejděte tooAmbari.</span><span class="sxs-lookup"><span data-stu-id="27b55-162">Go tooAmbari.</span></span> 
  2. <span data-ttu-id="27b55-163">Zobrazení přejděte tooTez (pod hello ikonu dlaždice v pravém horním rohu hello).</span><span class="sxs-lookup"><span data-stu-id="27b55-163">Go tooTez view (under hello tiles icon in hello upper-right corner).</span></span> 
  3. <span data-ttu-id="27b55-164">Vyberte hello chcete tooview DAG.</span><span class="sxs-lookup"><span data-stu-id="27b55-164">Select hello DAG you want tooview.</span></span>
  4. <span data-ttu-id="27b55-165">Vyberte **stahování dat**.</span><span class="sxs-lookup"><span data-stu-id="27b55-165">Select **Download data**.</span></span>

### <span data-ttu-id="27b55-166"><a name="additional-reading-end"></a>Další čtení</span><span class="sxs-lookup"><span data-stu-id="27b55-166"><a name="additional-reading-end"></a>Additional reading</span></span>

[<span data-ttu-id="27b55-167">Připojit tooan clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="27b55-167">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)






