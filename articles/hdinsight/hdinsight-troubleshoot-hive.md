---
title: "Řešení potíží Hive pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi na časté otázky týkající se práce s Apache Hive a Azure HDInsight."
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
ms.openlocfilehash: 53e9685458190efe6a586504721b8e7baadaed60
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a><span data-ttu-id="7fb73-104">Řešení potíží Hive pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7fb73-104">Troubleshoot Hive by using Azure HDInsight</span></span>

<span data-ttu-id="7fb73-105">Další informace o nejvyšší otázky a jejich řešení při práci s Apache Hive datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="7fb73-105">Learn about the top questions and their resolutions when working with Apache Hive payloads in Apache Ambari.</span></span>


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a><span data-ttu-id="7fb73-106">Jak exportovat metaúložiště Hive a importovat na jiný cluster</span><span class="sxs-lookup"><span data-stu-id="7fb73-106">How do I export a Hive metastore and import it on another cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="7fb73-107">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="7fb73-107">Resolution steps</span></span>

1. <span data-ttu-id="7fb73-108">Připojte se ke clusteru HDInsight pomocí klienta Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="7fb73-108">Connect to the HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="7fb73-109">Další informace najdete v tématu [další čtení](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="7fb73-109">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="7fb73-110">Na clusteru HDInsight, ze kterého chcete provést export metaúložiště spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-110">Run the following command on the HDInsight cluster from which you want to export the metastore:</span></span>

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  <span data-ttu-id="7fb73-111">Tento příkaz vytvoří soubor s názvem allatables.sql.</span><span class="sxs-lookup"><span data-stu-id="7fb73-111">This command generates a file named allatables.sql.</span></span>

3. <span data-ttu-id="7fb73-112">Zkopírujte soubor alltables.sql do nového clusteru HDInsight a pak spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-112">Copy the file alltables.sql to the new HDInsight cluster, and then run the following command:</span></span>

  ```apache
  hive -f alltables.sql
  ```

<span data-ttu-id="7fb73-113">Kód v kroky řešení předpokládá, že cesty k datům v novém clusteru jsou stejné jako cest k datům v původním clusteru.</span><span class="sxs-lookup"><span data-stu-id="7fb73-113">The code in the resolution steps assumes that data paths on the new cluster are the same as the data paths on the old cluster.</span></span> <span data-ttu-id="7fb73-114">Pokud cest k datům liší, můžete ručně upravit soubor generovaný alltables.sql tak, aby odrážela všechny změny.</span><span class="sxs-lookup"><span data-stu-id="7fb73-114">If the data paths are different, you can manually edit the generated alltables.sql file to reflect any changes.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="7fb73-115">Další čtení</span><span class="sxs-lookup"><span data-stu-id="7fb73-115">Additional reading</span></span>

- [<span data-ttu-id="7fb73-116">Připojení ke clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="7fb73-116">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a><span data-ttu-id="7fb73-117">Jak najít protokoly Hive v clusteru</span><span class="sxs-lookup"><span data-stu-id="7fb73-117">How do I locate Hive logs on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="7fb73-118">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="7fb73-118">Resolution steps</span></span>

1. <span data-ttu-id="7fb73-119">Připojte ke clusteru HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="7fb73-119">Connect to the HDInsight cluster by using SSH.</span></span> <span data-ttu-id="7fb73-120">Další informace najdete v tématu **další čtení**.</span><span class="sxs-lookup"><span data-stu-id="7fb73-120">For more information, see **Additional reading**.</span></span>

2. <span data-ttu-id="7fb73-121">Chcete-li zobrazit protokoly Hive klienta, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-121">To view Hive client logs, use the following command:</span></span>

  ```apache
  /tmp/<username>/hive.log 
  ```

3. <span data-ttu-id="7fb73-122">Pokud chcete zobrazit protokoly metaúložiště Hive, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-122">To view Hive metastore logs, use the following command:</span></span>

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. <span data-ttu-id="7fb73-123">Pokud chcete zobrazit protokoly Hiveserver, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-123">To view Hiveserver logs, use the following command:</span></span>

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a><span data-ttu-id="7fb73-124">Další čtení</span><span class="sxs-lookup"><span data-stu-id="7fb73-124">Additional reading</span></span>

- [<span data-ttu-id="7fb73-125">Připojení ke clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="7fb73-125">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a><span data-ttu-id="7fb73-126">Jak spuštění prostředí Hive s konkrétní konfigurací v clusteru</span><span class="sxs-lookup"><span data-stu-id="7fb73-126">How do I launch the Hive shell with specific configurations on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="7fb73-127">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="7fb73-127">Resolution steps</span></span>

1. <span data-ttu-id="7fb73-128">Zadejte pár klíč hodnota konfigurace při spuštění prostředí Hive.</span><span class="sxs-lookup"><span data-stu-id="7fb73-128">Specify a configuration key-value pair when you start the Hive shell.</span></span> <span data-ttu-id="7fb73-129">Další informace najdete v tématu [další čtení](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="7fb73-129">For more information, see [Additional reading](#additional-reading-end).</span></span>

  ```apache
  hive -hiveconf a=b 
  ```

2. <span data-ttu-id="7fb73-130">K zobrazení seznamu všech efektivní konfigurace v prostředí Hive, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-130">To list all effective configurations on Hive shell, use the following command:</span></span>

  ```apache
  hive> set;
  ```

  <span data-ttu-id="7fb73-131">Spuštění prostředí Hive se protokolování ladění na konzole například použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-131">For example, use the following command to start Hive shell with debug logging enabled on the console:</span></span>

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a><span data-ttu-id="7fb73-132">Další čtení</span><span class="sxs-lookup"><span data-stu-id="7fb73-132">Additional reading</span></span>

- [<span data-ttu-id="7fb73-133">Vlastnosti konfigurace Hive</span><span class="sxs-lookup"><span data-stu-id="7fb73-133">Hive configuration properties</span></span>](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <span data-ttu-id="7fb73-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Jak můžu analyzovat data Tez DAG v clusteru kritické cestě</span><span class="sxs-lookup"><span data-stu-id="7fb73-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>How do I analyze Tez DAG data on a cluster-critical path</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="7fb73-135">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="7fb73-135">Resolution steps</span></span>
 
1. <span data-ttu-id="7fb73-136">Pro analýzu Apache Tez necyklicky (DAG) na clusteru kritické graf, připojte ke clusteru HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="7fb73-136">To analyze an Apache Tez directed acyclic graph (DAG) on a cluster-critical graph, connect to the HDInsight cluster by using SSH.</span></span> <span data-ttu-id="7fb73-137">Další informace najdete v tématu [další čtení](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="7fb73-137">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="7fb73-138">Na příkazovém řádku spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-138">At a command prompt, run the following command:</span></span>
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. <span data-ttu-id="7fb73-139">Seznam dalších analyzátory, které lze použít k analýze Tez DAG, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-139">To list other analyzers that can be used to analyze Tez DAG, use the following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  <span data-ttu-id="7fb73-140">Ukázkový program je nutné zadat jako první argument.</span><span class="sxs-lookup"><span data-stu-id="7fb73-140">You must provide an example program as the first argument.</span></span>

  <span data-ttu-id="7fb73-141">Program platné názvy patří:</span><span class="sxs-lookup"><span data-stu-id="7fb73-141">Valid program names include:</span></span>
    - <span data-ttu-id="7fb73-142">**ContainerReuseAnalyzer**: tisk podrobností o opakované použití kontejneru v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-142">**ContainerReuseAnalyzer**: Print container reuse details in a DAG</span></span>
    - <span data-ttu-id="7fb73-143">**CriticalPath**: najít kritické cestu DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-143">**CriticalPath**: Find the critical path of a DAG</span></span>
    - <span data-ttu-id="7fb73-144">**LocalityAnalyzer**: tisk polohu podrobností v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-144">**LocalityAnalyzer**: Print locality details in a DAG</span></span>
    - <span data-ttu-id="7fb73-145">**ShuffleTimeAnalyzer**: analyzovat podrobnosti času náhodně v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-145">**ShuffleTimeAnalyzer**: Analyze the shuffle time details in a DAG</span></span>
    - <span data-ttu-id="7fb73-146">**SkewAnalyzer**: Analýza zkosení podrobnosti v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-146">**SkewAnalyzer**: Analyze the skew details in a DAG</span></span>
    - <span data-ttu-id="7fb73-147">**SlowNodeAnalyzer**: tisk podrobností uzlu v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-147">**SlowNodeAnalyzer**: Print node details in a DAG</span></span>
    - <span data-ttu-id="7fb73-148">**SlowTaskIdentifier**: tisk podrobnosti pomalé úlohy v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-148">**SlowTaskIdentifier**: Print slow task details in a DAG</span></span>
    - <span data-ttu-id="7fb73-149">**SlowestVertexAnalyzer**: tisk nejpomalejší vrchol podrobností v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-149">**SlowestVertexAnalyzer**: Print slowest vertex details in a DAG</span></span>
    - <span data-ttu-id="7fb73-150">**SpillAnalyzer**: tisk přepadového podrobnosti v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-150">**SpillAnalyzer**: Print spill details in a DAG</span></span>
    - <span data-ttu-id="7fb73-151">**TaskConcurrencyAnalyzer**: tisk souběžnosti podrobnosti úlohy v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-151">**TaskConcurrencyAnalyzer**: Print the task concurrency details in a DAG</span></span>
    - <span data-ttu-id="7fb73-152">**VertexLevelCriticalPathAnalyzer**: najít kritické cesty na úrovni vrchol v DAG</span><span class="sxs-lookup"><span data-stu-id="7fb73-152">**VertexLevelCriticalPathAnalyzer**: Find the critical path at vertex level in a DAG</span></span>


### <a name="additional-reading"></a><span data-ttu-id="7fb73-153">Další čtení</span><span class="sxs-lookup"><span data-stu-id="7fb73-153">Additional reading</span></span>

- [<span data-ttu-id="7fb73-154">Připojení ke clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="7fb73-154">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a><span data-ttu-id="7fb73-155">Jak lze stáhnout Tez DAG data z clusteru</span><span class="sxs-lookup"><span data-stu-id="7fb73-155">How do I download Tez DAG data from a cluster</span></span>


#### <a name="resolution-steps"></a><span data-ttu-id="7fb73-156">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="7fb73-156">Resolution steps</span></span>

<span data-ttu-id="7fb73-157">Ke shromažďování dat Tez DAG dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="7fb73-157">There are two ways to collect the Tez DAG data:</span></span>

- <span data-ttu-id="7fb73-158">Z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="7fb73-158">From the command line:</span></span>
 
    <span data-ttu-id="7fb73-159">Připojte ke clusteru HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="7fb73-159">Connect to the HDInsight cluster by using SSH.</span></span> <span data-ttu-id="7fb73-160">Na příkazovém řádku spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fb73-160">At the command prompt, run the following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- <span data-ttu-id="7fb73-161">Použijte zobrazení Ambari Tez:</span><span class="sxs-lookup"><span data-stu-id="7fb73-161">Use the Ambari Tez view:</span></span>
   
  1. <span data-ttu-id="7fb73-162">Přejděte na Ambari.</span><span class="sxs-lookup"><span data-stu-id="7fb73-162">Go to Ambari.</span></span> 
  2. <span data-ttu-id="7fb73-163">Přejděte do zobrazení Tez (pod ikonu dlaždice v pravém horním rohu).</span><span class="sxs-lookup"><span data-stu-id="7fb73-163">Go to Tez view (under the tiles icon in the upper-right corner).</span></span> 
  3. <span data-ttu-id="7fb73-164">Vyberte DAG, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="7fb73-164">Select the DAG you want to view.</span></span>
  4. <span data-ttu-id="7fb73-165">Vyberte **stahování dat**.</span><span class="sxs-lookup"><span data-stu-id="7fb73-165">Select **Download data**.</span></span>

### <span data-ttu-id="7fb73-166"><a name="additional-reading-end"></a>Další čtení</span><span class="sxs-lookup"><span data-stu-id="7fb73-166"><a name="additional-reading-end"></a>Additional reading</span></span>

[<span data-ttu-id="7fb73-167">Připojení ke clusteru HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="7fb73-167">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)






