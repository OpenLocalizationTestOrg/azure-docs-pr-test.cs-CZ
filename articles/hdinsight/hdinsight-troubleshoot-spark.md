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
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a>Řešení potíží Spark pomocí Azure HDInsight

Další informace o hello nejčastější problémy a jejich řešení při práci s Apache Spark datové části v Apache Ambari.

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a>Konfigurování aplikací Spark pomocí Ambari v clusterech

### <a name="resolution-steps"></a>Kroky řešení

Hello konfigurační hodnoty pro tento postup byly dříve nastavené v prostředí HDInsight. toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception). 

1. V seznamu hello clusterů vyberte **Spark2**.

    ![Vyberte cluster, ze seznamu](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. Vyberte hello **konfigurací** kartě.

    ![Vyberte kartu konfigurací hello](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. V seznamu hello konfigurací vyberte **výchozí hodnoty vlastní spark2**.

    ![Vyberte výchozí nastavení vlastní spark](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. Vyhledejte hello hodnotu nastavení, jako třeba tooadjust, **spark.executor.memory**. V takovém případě hello hodnotu **4608m** je příliš vysoká.

    ![Vyberte pole spark.executor.memory hello](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. Sada hello hodnota toohello doporučené nastavení. Hello hodnotu **2 048 m** se doporučuje pro toto nastavení.

    ![Změnit hodnotu too2048m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. Uložit hello hodnotu a uložte konfiguraci hello. Na panelu nástrojů hello, vyberte **Uložit**.

    ![Uložit hello nastavení a konfigurace](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    Budete upozorněni, pokud všechny konfigurace vyžadují pozornost. Hello položky a pak vyberte **přesto pokračovat**. 

    ![Vyberte přesto pokračovat](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    Napsat poznámku o hello změny konfigurace a potom vyberte **Uložit**.

    ![Zadejte poznámku o provedené změny hello](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. Vždy, když je uložit konfiguraci, zobrazí se výzva toorestart hello služby. Vyberte **restartujte**.

    ![Vyberte možnost restartování](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    Potvrďte hello restartování.

    ![Vyberte potvrďte restartujte](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    Můžete zkontrolovat hello procesy, které jsou spuštěny.

    ![Zkontrolujte spuštěných procesů](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. Můžete přidat konfigurace. V seznamu hello konfigurací vyberte **výchozí hodnoty vlastní spark2**a potom vyberte **přidat vlastnost**.

    ![Vyberte Přidat vlastnost](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. Definujte novou vlastnost. Pomocí dialogového okna pro konkrétní nastavení jako datový typ hello můžete definovat vlastnosti jediné. Nebo můžete definovat více vlastností pomocí jednu definici na každý řádek. 

    V tomto příkladu hello **spark.driver.memory** vlastnost je definován s hodnotou **4g**.

    ![Definovat nové vlastnosti](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. Hello konfiguraci uložit a potom restartujte službu hello, jak je popsáno v kroku 6 a 7.

Tyto změny jsou platné pro celý cluster, ale je možné přepsat při odesílání úlohy Spark hello.

### <a name="additional-reading"></a>Další čtení

[Odeslání úlohy Spark v clusterech prostředí HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Konfigurování aplikací Spark v clusterech pomocí poznámkového bloku Jupyter

### <a name="resolution-steps"></a>Kroky řešení

1. toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).

2. V první buňky hello poznámkového bloku Jupyter hello, po hello **%% konfigurace** – direktiva, zadejte hello Spark konfigurace v platném formátu JSON. Podle potřeby změnit hello skutečnými hodnotami:

    ![Přidání konfigurace](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Další čtení

[Odeslání úlohy Spark v clusterech prostředí HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a>Konfigurování aplikací Spark pomocí Livy v clusterech

### <a name="resolution-steps"></a>Kroky řešení

1. toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception). 

2. Odešlete tooLivy aplikací Spark hello pomocí klienta REST jako cURL. Použijte podobné toohello následující příkaz. Podle potřeby změnit hello skutečnými hodnotami:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Další čtení

[Odeslání úlohy Spark v clusterech prostředí HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a>Konfigurování odeslání spark aplikace pomocí Spark v clusterech

### <a name="resolution-steps"></a>Kroky řešení

1. toodetermine které Spark konfigurace potřebovat toobe sady a toowhat hodnoty, najdete v části [co způsobí, že Spark výjimka OutofMemoryError aplikace](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Spusťte prostředí spark pomocí podobné toohello následující příkaz. Podle potřeby změnit hello skutečná hodnota hello konfigurací: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Další čtení

[Odeslání úlohy Spark v clusterech prostředí HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Co způsobí, že Spark výjimka OutofMemoryError aplikace

### <a name="detailed-description"></a>Podrobný popis

Hello aplikací Spark selže s touto hello následující typy nezachycená výjimky:

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

### <a name="probable-cause"></a>Pravděpodobná příčina

Hello nejpravděpodobnější příčinou této výjimky je, že není dostatek paměti haldy je přidělen toohello Java virtuálních počítačů (JVMs). Tyto JVMs jsou spouštěny jako vykonavatelů nebo ovladače jako součást hello aplikací Spark. 

### <a name="resolution-steps"></a>Kroky řešení

1. Určete maximální velikost hello hello data hello Spark aplikace popisovače. Můžete nastavit odhad, založeny na maximální velikosti hello hello vstupních dat, hello mezilehlá data, která je vytvořena pomocí transformace hello vstupní data a hello výstupní data, která je vytvořena, když aplikace hello je další transformace hello mezilehlá data. Tento proces může být iterační, pokud nelze provádět počáteční formální odhad. 

2. Ujistěte se, že budete, že toouse nemá dostatek prostředků z hlediska paměť a počet jader aplikací Spark hello tooaccommodate clusteru HDInsight hello. Této služby můžete zjistit zobrazením hello clusteru metriky části hello uživatelském rozhraní YARN hello hodnoty z **paměti používá** vs. **Celkem paměti**, a **VCores používá** vs. **Celkový počet VCores**.

3. Nastavte následující Spark hello tooappropriate hodnoty konfigurace, které by neměl být delší než 90 % hello dostupnou paměť a počet jader. Hello hodnoty by měly být i v rámci požadavky velikost paměti hello hello Spark aplikace: 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    tooget hello celkové množství paměti používané všechny vykonavatelů, spusťte následující příkaz hello: 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    tooget hello celkové množství paměti používané hello ovladač, spusťte následující příkaz hello:
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a>Další čtení

- [Přehled správy paměti Spark](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [Ladění aplikací Spark v clusteru HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

