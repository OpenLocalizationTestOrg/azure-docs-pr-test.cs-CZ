---
title: "Řešení potíží Spark pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi na časté otázky týkající se práce s Apache Spark a Azure HDInsight."
keywords: "Azure HDInsight Spark, – nejčastější dotazy, řešení potíží s průvodce, běžné problémy, konfigurace aplikace, Ambari"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: cfed5f0f4f703821e83e3d365810c0e5ad22f035
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a><span data-ttu-id="f0cdf-104">Řešení potíží Spark pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0cdf-104">Troubleshoot Spark by using Azure HDInsight</span></span>

<span data-ttu-id="f0cdf-105">Další informace o hlavních problémů a jejich řešení při práci s Apache Spark datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-105">Learn about the top issues and their resolutions when working with Apache Spark payloads in Apache Ambari.</span></span>

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a><span data-ttu-id="f0cdf-106">Konfigurování aplikací Spark pomocí Ambari v clusterech</span><span class="sxs-lookup"><span data-stu-id="f0cdf-106">How do I configure a Spark application by using Ambari on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="f0cdf-107">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-107">Resolution steps</span></span>

<span data-ttu-id="f0cdf-108">V prostředí HDInsight byly dříve nastavené hodnoty konfigurace pro tento postup.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-108">The configuration values for this procedure were previously set in HDInsight.</span></span> <span data-ttu-id="f0cdf-109">Chcete-li určit, které Spark konfigurace muset nastavit a jaké hodnoty, najdete v tématu [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="f0cdf-109">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

1. <span data-ttu-id="f0cdf-110">Vyberte v seznamu clusterů **Spark2**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-110">In the list of clusters, select **Spark2**.</span></span>

    ![Vyberte cluster, ze seznamu](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. <span data-ttu-id="f0cdf-112">Vyberte **konfigurací** kartě.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-112">Select the **Configs** tab.</span></span>

    ![Vyberte kartu konfigurací](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. <span data-ttu-id="f0cdf-114">Vyberte v seznamu konfigurací **výchozí hodnoty vlastní spark2**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-114">In the list of configurations, select **Custom-spark2-defaults**.</span></span>

    ![Vyberte výchozí nastavení vlastní spark](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. <span data-ttu-id="f0cdf-116">Vyhledejte nastavení hodnoty, které je nutné upravit, jako například **spark.executor.memory**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-116">Look for the value setting that you need to adjust, such as **spark.executor.memory**.</span></span> <span data-ttu-id="f0cdf-117">V tomto případě hodnotu **4608m** je příliš vysoká.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-117">In this case, the value of **4608m** is too high.</span></span>

    ![Vyberte pole spark.executor.memory](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. <span data-ttu-id="f0cdf-119">Nastavte hodnotu na doporučené nastavení.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-119">Set the value to the recommended setting.</span></span> <span data-ttu-id="f0cdf-120">Hodnota **2 048 m** se doporučuje pro toto nastavení.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-120">The value **2048m** is recommended for this setting.</span></span>

    ![Změňte hodnotu na 2 048 m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. <span data-ttu-id="f0cdf-122">Uložit hodnotu a pak konfiguraci uložte.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-122">Save the value, and then save the configuration.</span></span> <span data-ttu-id="f0cdf-123">Na panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-123">On the toolbar, select **Save**.</span></span>

    ![Uložte nastavení a konfigurace](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    <span data-ttu-id="f0cdf-125">Budete upozorněni, pokud všechny konfigurace vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-125">You are notified if any configurations need attention.</span></span> <span data-ttu-id="f0cdf-126">Položky a pak vyberte **přesto pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-126">Note the items, and then select **Proceed Anyway**.</span></span> 

    ![Vyberte přesto pokračovat](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    <span data-ttu-id="f0cdf-128">Napsat poznámku o změny konfigurace a potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-128">Write a note about the configuration changes, and then select **Save**.</span></span>

    ![Zadejte poznámku o provedené změny](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. <span data-ttu-id="f0cdf-130">Vždy, když je uložit konfiguraci, zobrazí se výzva k restartování služby.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-130">Whenever a configuration is saved, you are prompted to restart the service.</span></span> <span data-ttu-id="f0cdf-131">Vyberte **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-131">Select **Restart**.</span></span>

    ![Vyberte možnost restartování](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    <span data-ttu-id="f0cdf-133">Potvrďte restartování.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-133">Confirm the restart.</span></span>

    ![Vyberte potvrďte restartujte](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    <span data-ttu-id="f0cdf-135">Můžete zkontrolovat procesy, které jsou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-135">You can review the processes that are running.</span></span>

    ![Zkontrolujte spuštěných procesů](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. <span data-ttu-id="f0cdf-137">Můžete přidat konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-137">You can add configurations.</span></span> <span data-ttu-id="f0cdf-138">Vyberte v seznamu konfigurací **výchozí hodnoty vlastní spark2**a potom vyberte **přidat vlastnost**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-138">In the list of configurations, select **Custom-spark2-defaults**, and then select **Add Property**.</span></span>

    ![Vyberte Přidat vlastnost](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. <span data-ttu-id="f0cdf-140">Definujte novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-140">Define a new property.</span></span> <span data-ttu-id="f0cdf-141">Pomocí dialogového okna pro konkrétní nastavení, jako je datový typ, můžete definovat vlastnosti jediné.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-141">You can define a single property by using a dialog box for specific settings such as the data type.</span></span> <span data-ttu-id="f0cdf-142">Nebo můžete definovat více vlastností pomocí jednu definici na každý řádek.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-142">Or, you can define multiple properties by using one definition per line.</span></span> 

    <span data-ttu-id="f0cdf-143">V tomto příkladu **spark.driver.memory** vlastnost je definován s hodnotou **4g**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-143">In this example, the **spark.driver.memory** property is defined with a value of **4g**.</span></span>

    ![Definovat nové vlastnosti](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. <span data-ttu-id="f0cdf-145">Uložte nastavení a potom restartujte službu, jak je popsáno v kroku 6 a 7.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-145">Save the configuration, and then restart the service as described in steps 6 and 7.</span></span>

<span data-ttu-id="f0cdf-146">Tyto změny jsou platné pro celý cluster, ale je možné přepsat při odesílání úlohy Spark.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-146">These changes are cluster-wide but can be overridden when you submit the Spark job.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="f0cdf-147">Další čtení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-147">Additional reading</span></span>

[<span data-ttu-id="f0cdf-148">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0cdf-148">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a><span data-ttu-id="f0cdf-149">Konfigurování aplikací Spark v clusterech pomocí poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="f0cdf-149">How do I configure a Spark application by using a Jupyter notebook on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="f0cdf-150">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-150">Resolution steps</span></span>

1. <span data-ttu-id="f0cdf-151">Chcete-li určit, které Spark konfigurace muset nastavit a jaké hodnoty, najdete v tématu [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="f0cdf-151">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="f0cdf-152">V první buňky blok Jupyter po **%% konfigurace** – direktiva, zadejte konfigurace Spark v platném formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-152">In the first cell of the Jupyter notebook, after the **%%configure** directive, specify the Spark configurations in valid JSON format.</span></span> <span data-ttu-id="f0cdf-153">Podle potřeby změňte skutečnými hodnotami:</span><span class="sxs-lookup"><span data-stu-id="f0cdf-153">Change the actual values as necessary:</span></span>

    ![Přidání konfigurace](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a><span data-ttu-id="f0cdf-155">Další čtení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-155">Additional reading</span></span>

[<span data-ttu-id="f0cdf-156">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0cdf-156">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a><span data-ttu-id="f0cdf-157">Konfigurování aplikací Spark pomocí Livy v clusterech</span><span class="sxs-lookup"><span data-stu-id="f0cdf-157">How do I configure a Spark application by using Livy on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="f0cdf-158">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-158">Resolution steps</span></span>

1. <span data-ttu-id="f0cdf-159">Chcete-li určit, které Spark konfigurace muset nastavit a jaké hodnoty, najdete v tématu [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="f0cdf-159">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

2. <span data-ttu-id="f0cdf-160">Odesílání aplikací Spark pro Livy pomocí klienta REST jako cURL.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-160">Submit the Spark application to Livy by using a REST client like cURL.</span></span> <span data-ttu-id="f0cdf-161">Použijte příkaz podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-161">Use a command similar to the following.</span></span> <span data-ttu-id="f0cdf-162">Podle potřeby změňte skutečnými hodnotami:</span><span class="sxs-lookup"><span data-stu-id="f0cdf-162">Change the actual values as necessary:</span></span>

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a><span data-ttu-id="f0cdf-163">Další čtení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-163">Additional reading</span></span>

[<span data-ttu-id="f0cdf-164">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0cdf-164">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a><span data-ttu-id="f0cdf-165">Konfigurování odeslání spark aplikace pomocí Spark v clusterech</span><span class="sxs-lookup"><span data-stu-id="f0cdf-165">How do I configure a Spark application by using spark-submit on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="f0cdf-166">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-166">Resolution steps</span></span>

1. <span data-ttu-id="f0cdf-167">Chcete-li určit, které Spark konfigurace muset nastavit a jaké hodnoty, najdete v tématu [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="f0cdf-167">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="f0cdf-168">Spusťte prostředí spark pomocí příkazu, který je podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-168">Launch spark-shell by using a command similar to the following.</span></span> <span data-ttu-id="f0cdf-169">Podle potřeby změňte skutečné hodnoty konfigurace:</span><span class="sxs-lookup"><span data-stu-id="f0cdf-169">Change the actual value of the configurations as necessary:</span></span> 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a><span data-ttu-id="f0cdf-170">Další čtení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-170">Additional reading</span></span>

[<span data-ttu-id="f0cdf-171">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0cdf-171">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a><span data-ttu-id="f0cdf-172">Co způsobí, že Spark výjimka OutofMemoryError aplikace</span><span class="sxs-lookup"><span data-stu-id="f0cdf-172">What causes a Spark application OutofMemoryError exception</span></span>

### <a name="detailed-description"></a><span data-ttu-id="f0cdf-173">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="f0cdf-173">Detailed description</span></span>

<span data-ttu-id="f0cdf-174">Aplikace Spark selže s následujícími typy nezachycená výjimky:</span><span class="sxs-lookup"><span data-stu-id="f0cdf-174">The Spark application fails, with the following types of uncaught exceptions:</span></span>

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a><span data-ttu-id="f0cdf-175">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="f0cdf-175">Probable cause</span></span>

<span data-ttu-id="f0cdf-176">Nejpravděpodobnější příčinou této výjimky je, že není dostatek paměti haldy je přidělen k virtuálním počítačům Java (JVMs).</span><span class="sxs-lookup"><span data-stu-id="f0cdf-176">The most likely cause of this exception is that not enough heap memory is allocated to the Java virtual machines (JVMs).</span></span> <span data-ttu-id="f0cdf-177">Tyto JVMs jsou spouštěny jako vykonavatelů nebo ovladače jako součást aplikací Spark.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-177">These JVMs are launched as executors or drivers as part of the Spark application.</span></span> 

### <a name="resolution-steps"></a><span data-ttu-id="f0cdf-178">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-178">Resolution steps</span></span>

1. <span data-ttu-id="f0cdf-179">Určit maximální velikost dat Spark aplikace zpracovává.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-179">Determine the maximum size of the data the Spark application handles.</span></span> <span data-ttu-id="f0cdf-180">Můžete nastavit odhad, založené na maximální velikost vstupních dat, mezilehlá data, která je produkovaný transformace dat vstupní a výstupní data, která je vytvořena, když aplikace je další transformace mezilehlá data.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-180">You can make a guess based on the maximum size of the input data, the intermediate data that's produced by transforming the input data, and the output data that's produced when the application is further transforming the intermediate data.</span></span> <span data-ttu-id="f0cdf-181">Tento proces může být iterační, pokud nelze provádět počáteční formální odhad.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-181">This process can be an iterative if you can't make an initial formal guess.</span></span> 

2. <span data-ttu-id="f0cdf-182">Ujistěte se, že cluster HDInsight, který se chystáte použít má dostatek paměti a počet jader, aby dokázala pojmout aplikací Spark zdroje.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-182">Make sure that the HDInsight cluster that you're going to use has enough resources in terms of memory and cores to accommodate the Spark application.</span></span> <span data-ttu-id="f0cdf-183">Této služby můžete zjistit zobrazením části clusteru metriky rozhraní YARN pro hodnoty **paměti používá** vs. **Celkem paměti**, a **VCores používá** vs. **Celkový počet VCores**.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-183">You can determine this by viewing the cluster metrics section of the YARN UI for the values of **Memory Used** vs. **Memory Total**, and **VCores Used** vs. **VCores Total**.</span></span>

3. <span data-ttu-id="f0cdf-184">Nastavte následující konfigurace Spark na odpovídající hodnoty, které by neměl být delší než 90 % dostupné paměti a počet jader.</span><span class="sxs-lookup"><span data-stu-id="f0cdf-184">Set the following Spark configurations to appropriate values, which should not exceed 90% of the available memory and cores.</span></span> <span data-ttu-id="f0cdf-185">Hodnoty by měla být i v rámci požadavky na paměť Spark aplikace:</span><span class="sxs-lookup"><span data-stu-id="f0cdf-185">The values should be well within the memory requirements of the Spark application:</span></span> 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    <span data-ttu-id="f0cdf-186">Pokud chcete získat celkové množství paměti používané všechny vykonavatelů, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f0cdf-186">To get the total memory used by all executors, run the following command:</span></span> 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    <span data-ttu-id="f0cdf-187">Pokud chcete získat celkové množství paměti používané ovladače, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f0cdf-187">To get the total memory used by the driver, run the following command:</span></span>
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a><span data-ttu-id="f0cdf-188">Další čtení</span><span class="sxs-lookup"><span data-stu-id="f0cdf-188">Additional reading</span></span>

- [<span data-ttu-id="f0cdf-189">Přehled správy paměti Spark</span><span class="sxs-lookup"><span data-stu-id="f0cdf-189">Spark memory management overview</span></span>](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [<span data-ttu-id="f0cdf-190">Ladění aplikací Spark v clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0cdf-190">Debug a Spark application on an HDInsight cluster</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

