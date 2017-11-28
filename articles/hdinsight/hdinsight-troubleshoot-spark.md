---
title: "aaaTroubleshoot Spark pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi toocommon dotazy týkající se práce s Apache Spark a Azure HDInsight."
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
ms.openlocfilehash: c9f910daf295462238a3143ae2589db6d383097f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a><span data-ttu-id="e96f0-104">Řešení potíží Spark pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="e96f0-104">Troubleshoot Spark by using Azure HDInsight</span></span>

<span data-ttu-id="e96f0-105">Další informace o hello nejčastější problémy a jejich řešení při práci s Apache Spark datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="e96f0-105">Learn about hello top issues and their resolutions when working with Apache Spark payloads in Apache Ambari.</span></span>

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a><span data-ttu-id="e96f0-106">Konfigurování aplikací Spark pomocí Ambari v clusterech</span><span class="sxs-lookup"><span data-stu-id="e96f0-106">How do I configure a Spark application by using Ambari on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="e96f0-107">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="e96f0-107">Resolution steps</span></span>

<span data-ttu-id="e96f0-108">Hello konfigurační hodnoty pro tento postup byly dříve nastavené v prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e96f0-108">hello configuration values for this procedure were previously set in HDInsight.</span></span> <span data-ttu-id="e96f0-109">toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="e96f0-109">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

1. <span data-ttu-id="e96f0-110">V seznamu hello clusterů vyberte **Spark2**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-110">In hello list of clusters, select **Spark2**.</span></span>

    ![Vyberte cluster, ze seznamu](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. <span data-ttu-id="e96f0-112">Vyberte hello **konfigurací** kartě.</span><span class="sxs-lookup"><span data-stu-id="e96f0-112">Select hello **Configs** tab.</span></span>

    ![Vyberte kartu konfigurací hello](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. <span data-ttu-id="e96f0-114">V seznamu hello konfigurací vyberte **výchozí hodnoty vlastní spark2**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-114">In hello list of configurations, select **Custom-spark2-defaults**.</span></span>

    ![Vyberte výchozí nastavení vlastní spark](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. <span data-ttu-id="e96f0-116">Vyhledejte hello hodnotu nastavení, jako třeba tooadjust, **spark.executor.memory**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-116">Look for hello value setting that you need tooadjust, such as **spark.executor.memory**.</span></span> <span data-ttu-id="e96f0-117">V takovém případě hello hodnotu **4608m** je příliš vysoká.</span><span class="sxs-lookup"><span data-stu-id="e96f0-117">In this case, hello value of **4608m** is too high.</span></span>

    ![Vyberte pole spark.executor.memory hello](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. <span data-ttu-id="e96f0-119">Sada hello hodnota toohello doporučené nastavení.</span><span class="sxs-lookup"><span data-stu-id="e96f0-119">Set hello value toohello recommended setting.</span></span> <span data-ttu-id="e96f0-120">Hello hodnotu **2 048 m** se doporučuje pro toto nastavení.</span><span class="sxs-lookup"><span data-stu-id="e96f0-120">hello value **2048m** is recommended for this setting.</span></span>

    ![Změnit hodnotu too2048m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. <span data-ttu-id="e96f0-122">Uložit hello hodnotu a uložte konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="e96f0-122">Save hello value, and then save hello configuration.</span></span> <span data-ttu-id="e96f0-123">Na panelu nástrojů hello, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-123">On hello toolbar, select **Save**.</span></span>

    ![Uložit hello nastavení a konfigurace](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    <span data-ttu-id="e96f0-125">Budete upozorněni, pokud všechny konfigurace vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="e96f0-125">You are notified if any configurations need attention.</span></span> <span data-ttu-id="e96f0-126">Hello položky a pak vyberte **přesto pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-126">Note hello items, and then select **Proceed Anyway**.</span></span> 

    ![Vyberte přesto pokračovat](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    <span data-ttu-id="e96f0-128">Napsat poznámku o hello změny konfigurace a potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-128">Write a note about hello configuration changes, and then select **Save**.</span></span>

    ![Zadejte poznámku o provedené změny hello](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. <span data-ttu-id="e96f0-130">Vždy, když je uložit konfiguraci, zobrazí se výzva toorestart hello služby.</span><span class="sxs-lookup"><span data-stu-id="e96f0-130">Whenever a configuration is saved, you are prompted toorestart hello service.</span></span> <span data-ttu-id="e96f0-131">Vyberte **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-131">Select **Restart**.</span></span>

    ![Vyberte možnost restartování](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    <span data-ttu-id="e96f0-133">Potvrďte hello restartování.</span><span class="sxs-lookup"><span data-stu-id="e96f0-133">Confirm hello restart.</span></span>

    ![Vyberte potvrďte restartujte](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    <span data-ttu-id="e96f0-135">Můžete zkontrolovat hello procesy, které jsou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="e96f0-135">You can review hello processes that are running.</span></span>

    ![Zkontrolujte spuštěných procesů](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. <span data-ttu-id="e96f0-137">Můžete přidat konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e96f0-137">You can add configurations.</span></span> <span data-ttu-id="e96f0-138">V seznamu hello konfigurací vyberte **výchozí hodnoty vlastní spark2**a potom vyberte **přidat vlastnost**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-138">In hello list of configurations, select **Custom-spark2-defaults**, and then select **Add Property**.</span></span>

    ![Vyberte Přidat vlastnost](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. <span data-ttu-id="e96f0-140">Definujte novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e96f0-140">Define a new property.</span></span> <span data-ttu-id="e96f0-141">Pomocí dialogového okna pro konkrétní nastavení jako datový typ hello můžete definovat vlastnosti jediné.</span><span class="sxs-lookup"><span data-stu-id="e96f0-141">You can define a single property by using a dialog box for specific settings such as hello data type.</span></span> <span data-ttu-id="e96f0-142">Nebo můžete definovat více vlastností pomocí jednu definici na každý řádek.</span><span class="sxs-lookup"><span data-stu-id="e96f0-142">Or, you can define multiple properties by using one definition per line.</span></span> 

    <span data-ttu-id="e96f0-143">V tomto příkladu hello **spark.driver.memory** vlastnost je definován s hodnotou **4g**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-143">In this example, hello **spark.driver.memory** property is defined with a value of **4g**.</span></span>

    ![Definovat nové vlastnosti](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. <span data-ttu-id="e96f0-145">Hello konfiguraci uložit a potom restartujte službu hello, jak je popsáno v kroku 6 a 7.</span><span class="sxs-lookup"><span data-stu-id="e96f0-145">Save hello configuration, and then restart hello service as described in steps 6 and 7.</span></span>

<span data-ttu-id="e96f0-146">Tyto změny jsou platné pro celý cluster, ale je možné přepsat při odesílání úlohy Spark hello.</span><span class="sxs-lookup"><span data-stu-id="e96f0-146">These changes are cluster-wide but can be overridden when you submit hello Spark job.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="e96f0-147">Další čtení</span><span class="sxs-lookup"><span data-stu-id="e96f0-147">Additional reading</span></span>

[<span data-ttu-id="e96f0-148">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="e96f0-148">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a><span data-ttu-id="e96f0-149">Konfigurování aplikací Spark v clusterech pomocí poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="e96f0-149">How do I configure a Spark application by using a Jupyter notebook on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="e96f0-150">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="e96f0-150">Resolution steps</span></span>

1. <span data-ttu-id="e96f0-151">toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="e96f0-151">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="e96f0-152">V první buňky hello poznámkového bloku Jupyter hello, po hello **%% konfigurace** – direktiva, zadejte hello Spark konfigurace v platném formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="e96f0-152">In hello first cell of hello Jupyter notebook, after hello **%%configure** directive, specify hello Spark configurations in valid JSON format.</span></span> <span data-ttu-id="e96f0-153">Podle potřeby změnit hello skutečnými hodnotami:</span><span class="sxs-lookup"><span data-stu-id="e96f0-153">Change hello actual values as necessary:</span></span>

    ![Přidání konfigurace](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a><span data-ttu-id="e96f0-155">Další čtení</span><span class="sxs-lookup"><span data-stu-id="e96f0-155">Additional reading</span></span>

[<span data-ttu-id="e96f0-156">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="e96f0-156">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a><span data-ttu-id="e96f0-157">Konfigurování aplikací Spark pomocí Livy v clusterech</span><span class="sxs-lookup"><span data-stu-id="e96f0-157">How do I configure a Spark application by using Livy on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="e96f0-158">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="e96f0-158">Resolution steps</span></span>

1. <span data-ttu-id="e96f0-159">toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="e96f0-159">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

2. <span data-ttu-id="e96f0-160">Odešlete tooLivy aplikací Spark hello pomocí klienta REST jako cURL.</span><span class="sxs-lookup"><span data-stu-id="e96f0-160">Submit hello Spark application tooLivy by using a REST client like cURL.</span></span> <span data-ttu-id="e96f0-161">Použijte podobné toohello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e96f0-161">Use a command similar toohello following.</span></span> <span data-ttu-id="e96f0-162">Podle potřeby změnit hello skutečnými hodnotami:</span><span class="sxs-lookup"><span data-stu-id="e96f0-162">Change hello actual values as necessary:</span></span>

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a><span data-ttu-id="e96f0-163">Další čtení</span><span class="sxs-lookup"><span data-stu-id="e96f0-163">Additional reading</span></span>

[<span data-ttu-id="e96f0-164">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="e96f0-164">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a><span data-ttu-id="e96f0-165">Konfigurování odeslání spark aplikace pomocí Spark v clusterech</span><span class="sxs-lookup"><span data-stu-id="e96f0-165">How do I configure a Spark application by using spark-submit on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="e96f0-166">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="e96f0-166">Resolution steps</span></span>

1. <span data-ttu-id="e96f0-167">toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="e96f0-167">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="e96f0-168">Spusťte prostředí spark pomocí podobné toohello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e96f0-168">Launch spark-shell by using a command similar toohello following.</span></span> <span data-ttu-id="e96f0-169">Podle potřeby změnit hello skutečná hodnota hello konfigurací:</span><span class="sxs-lookup"><span data-stu-id="e96f0-169">Change hello actual value of hello configurations as necessary:</span></span> 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a><span data-ttu-id="e96f0-170">Další čtení</span><span class="sxs-lookup"><span data-stu-id="e96f0-170">Additional reading</span></span>

[<span data-ttu-id="e96f0-171">Odeslání úlohy Spark v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="e96f0-171">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a><span data-ttu-id="e96f0-172">Co způsobí, že Spark výjimka OutofMemoryError aplikace</span><span class="sxs-lookup"><span data-stu-id="e96f0-172">What causes a Spark application OutofMemoryError exception</span></span>

### <a name="detailed-description"></a><span data-ttu-id="e96f0-173">Podrobný popis</span><span class="sxs-lookup"><span data-stu-id="e96f0-173">Detailed description</span></span>

<span data-ttu-id="e96f0-174">Hello aplikací Spark selže s touto hello následující typy nezachycená výjimky:</span><span class="sxs-lookup"><span data-stu-id="e96f0-174">hello Spark application fails, with hello following types of uncaught exceptions:</span></span>

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

### <a name="probable-cause"></a><span data-ttu-id="e96f0-175">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="e96f0-175">Probable cause</span></span>

<span data-ttu-id="e96f0-176">Hello nejpravděpodobnější příčinou této výjimky je, že není dostatek paměti haldy je přidělen toohello Java virtuálních počítačů (JVMs).</span><span class="sxs-lookup"><span data-stu-id="e96f0-176">hello most likely cause of this exception is that not enough heap memory is allocated toohello Java virtual machines (JVMs).</span></span> <span data-ttu-id="e96f0-177">Tyto JVMs jsou spouštěny jako vykonavatelů nebo ovladače jako součást hello aplikací Spark.</span><span class="sxs-lookup"><span data-stu-id="e96f0-177">These JVMs are launched as executors or drivers as part of hello Spark application.</span></span> 

### <a name="resolution-steps"></a><span data-ttu-id="e96f0-178">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="e96f0-178">Resolution steps</span></span>

1. <span data-ttu-id="e96f0-179">Určete maximální velikost hello hello data hello Spark aplikace popisovače.</span><span class="sxs-lookup"><span data-stu-id="e96f0-179">Determine hello maximum size of hello data hello Spark application handles.</span></span> <span data-ttu-id="e96f0-180">Můžete nastavit odhad, založeny na maximální velikosti hello hello vstupních dat, hello mezilehlá data, která je vytvořena pomocí transformace hello vstupní data a hello výstupní data, která je vytvořena, když aplikace hello je další transformace hello mezilehlá data.</span><span class="sxs-lookup"><span data-stu-id="e96f0-180">You can make a guess based on hello maximum size of hello input data, hello intermediate data that's produced by transforming hello input data, and hello output data that's produced when hello application is further transforming hello intermediate data.</span></span> <span data-ttu-id="e96f0-181">Tento proces může být iterační, pokud nelze provádět počáteční formální odhad.</span><span class="sxs-lookup"><span data-stu-id="e96f0-181">This process can be an iterative if you can't make an initial formal guess.</span></span> 

2. <span data-ttu-id="e96f0-182">Ujistěte se, že budete, že toouse nemá dostatek prostředků z hlediska paměť a počet jader aplikací Spark hello tooaccommodate clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="e96f0-182">Make sure that hello HDInsight cluster that you're going toouse has enough resources in terms of memory and cores tooaccommodate hello Spark application.</span></span> <span data-ttu-id="e96f0-183">Této služby můžete zjistit zobrazením hello clusteru metriky části hello uživatelském rozhraní YARN hello hodnoty z **paměti používá** vs. **Celkem paměti**, a **VCores používá** vs. **Celkový počet VCores**.</span><span class="sxs-lookup"><span data-stu-id="e96f0-183">You can determine this by viewing hello cluster metrics section of hello YARN UI for hello values of **Memory Used** vs. **Memory Total**, and **VCores Used** vs. **VCores Total**.</span></span>

3. <span data-ttu-id="e96f0-184">Nastavte následující Spark hello tooappropriate hodnoty konfigurace, které by neměl být delší než 90 % hello dostupnou paměť a počet jader.</span><span class="sxs-lookup"><span data-stu-id="e96f0-184">Set hello following Spark configurations tooappropriate values, which should not exceed 90% of hello available memory and cores.</span></span> <span data-ttu-id="e96f0-185">Hello hodnoty by měly být i v rámci požadavky velikost paměti hello hello Spark aplikace:</span><span class="sxs-lookup"><span data-stu-id="e96f0-185">hello values should be well within hello memory requirements of hello Spark application:</span></span> 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    <span data-ttu-id="e96f0-186">tooget hello celkové množství paměti používané všechny vykonavatelů, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e96f0-186">tooget hello total memory used by all executors, run hello following command:</span></span> 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    <span data-ttu-id="e96f0-187">tooget hello celkové množství paměti používané hello ovladač, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e96f0-187">tooget hello total memory used by hello driver, run hello following command:</span></span>
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a><span data-ttu-id="e96f0-188">Další čtení</span><span class="sxs-lookup"><span data-stu-id="e96f0-188">Additional reading</span></span>

- [<span data-ttu-id="e96f0-189">Přehled správy paměti Spark</span><span class="sxs-lookup"><span data-stu-id="e96f0-189">Spark memory management overview</span></span>](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [<span data-ttu-id="e96f0-190">Ladění aplikací Spark v clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="e96f0-190">Debug a Spark application on an HDInsight cluster</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

