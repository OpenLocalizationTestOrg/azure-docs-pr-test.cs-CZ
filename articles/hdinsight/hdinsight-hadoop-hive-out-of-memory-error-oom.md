---
title: "Opravte Hive nedostatek paměti v Azure HDInsight | Microsoft Docs"
description: "Opravte Hive nedostatek paměti v HDInsight. Scénář zákazníka je dotaz napříč mnoha velké tabulky."
keywords: "Nedostatek paměti chyby, OOM, Hive nastavení"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 7bce3dff-9825-4fa0-a568-c52a9f7d1dad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/17/2017
ms.author: jgao
ms.openlocfilehash: da1247070ade11f78b505524f5e970e18eb16d10
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a><span data-ttu-id="56805-105">Opravte Hive nedostatek paměti v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="56805-105">Fix a Hive out of memory error in Azure HDInsight</span></span>

<span data-ttu-id="56805-106">Zjistěte, jak opravit Hive nedostatek paměti při zpracování velkých tabulky nakonfigurováním nastavení paměti Hive.</span><span class="sxs-lookup"><span data-stu-id="56805-106">Learn how to fix a Hive out of memory error when process large tables by configuring Hive memory settings.</span></span>

## <a name="run-hive-query-against-large-tables"></a><span data-ttu-id="56805-107">Spusťte dotaz Hive na velké tabulky</span><span class="sxs-lookup"><span data-stu-id="56805-107">Run Hive query against large tables</span></span>

<span data-ttu-id="56805-108">Zákazník spustili dotazů Hive:</span><span class="sxs-lookup"><span data-stu-id="56805-108">A customer ran a Hive query:</span></span>

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

<span data-ttu-id="56805-109">Některé drobné odlišnosti tohoto dotazu:</span><span class="sxs-lookup"><span data-stu-id="56805-109">Some nuances of this query:</span></span>

* <span data-ttu-id="56805-110">T1 je alias na velkých tabulku tabulky1, který má spoustu typy sloupců řetězec.</span><span class="sxs-lookup"><span data-stu-id="56805-110">T1 is an alias to a big table, TABLE1, which has lots of STRING column types.</span></span>
* <span data-ttu-id="56805-111">Ostatní tabulky není, jsou velký, ale máte mnoho sloupců.</span><span class="sxs-lookup"><span data-stu-id="56805-111">Other tables are not that big but do have many columns.</span></span>
* <span data-ttu-id="56805-112">Všechny tabulky jsou propojení mezi sebou, v některých případech s více sloupců v tabulky1 a ostatní.</span><span class="sxs-lookup"><span data-stu-id="56805-112">All tables are joining each other, in some cases with multiple columns in TABLE1 and others.</span></span>

<span data-ttu-id="56805-113">Dotaz Hive trvalo 26 minut na dokončení na 24 uzlu clusteru A3 HDInsight.</span><span class="sxs-lookup"><span data-stu-id="56805-113">The Hive query took 26 minutes to finish on a 24 node A3 HDInsight cluster.</span></span> <span data-ttu-id="56805-114">Zákazník si všimli následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="56805-114">The customer noticed the following warning messages:</span></span>

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

<span data-ttu-id="56805-115">Pomocí modulu Tez.</span><span class="sxs-lookup"><span data-stu-id="56805-115">By using the Tez execution engine.</span></span> <span data-ttu-id="56805-116">Stejný dotaz spustili 15 minut a pak vrátil následující chybovou zprávu:</span><span class="sxs-lookup"><span data-stu-id="56805-116">The same query ran for 15 minutes, and then threw the following error:</span></span>

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

<span data-ttu-id="56805-117">Chyba zůstává při použití větší virtuální počítač (například D12).</span><span class="sxs-lookup"><span data-stu-id="56805-117">The error remains when using a bigger virtual machine (for example, D12).</span></span>


## <a name="debug-the-out-of-memory-error"></a><span data-ttu-id="56805-118">Ladění mimo Chyba paměti</span><span class="sxs-lookup"><span data-stu-id="56805-118">Debug the out of memory error</span></span>

<span data-ttu-id="56805-119">Jeden z problémů, příčinou chyba nedostatku paměti bylo nalezeno naše podporu a vývojové týmy společně [známý problém popsaný v Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span><span class="sxs-lookup"><span data-stu-id="56805-119">Our support and engineering teams together found one of the issues causing the out of memory error was a [known issue described in the Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span></span>

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

<span data-ttu-id="56805-120">**Hive.auto.convert.join.noconditionaltask** ve hive-site.xml souboru byla nastavena na **true**:</span><span class="sxs-lookup"><span data-stu-id="56805-120">The **hive.auto.convert.join.noconditionaltask** in the hive-site.xml file was set to **true**:</span></span>

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
              If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
              specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
      </property>

<span data-ttu-id="56805-121">Je pravděpodobné, mapy připojení byl příčinou haldy prostoru Java naše chyby paměti.</span><span class="sxs-lookup"><span data-stu-id="56805-121">It is likely map join was the cause of the Java Heap Space our of memory error.</span></span> <span data-ttu-id="56805-122">Jak je popsáno v příspěvku na blogu [nastavení paměti Hadoop Yarn v HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), při použití Tez je modul pro vykonání halda místo využité ve skutečnosti patří ke kontejneru Tez.</span><span class="sxs-lookup"><span data-stu-id="56805-122">As explained in the blog post [Hadoop Yarn memory settings in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), when Tez execution engine is used the heap space used actually belongs to the Tez container.</span></span> <span data-ttu-id="56805-123">Viz následující obrázek popisující paměti kontejneru Tez.</span><span class="sxs-lookup"><span data-stu-id="56805-123">See the following image describing the Tez container memory.</span></span>

![Diagram paměti kontejneru tez: Hive nedostatek paměti](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

<span data-ttu-id="56805-125">Jak v příspěvku blogu naznačuje, následující nastavení dva paměti definování paměti kontejner pro halda: **hive.tez.container.size** a **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="56805-125">As the blog post suggests, the following two memory settings define the container memory for the heap: **hive.tez.container.size** and **hive.tez.java.opts**.</span></span> <span data-ttu-id="56805-126">Z našich zkušeností se výjimka paměti neznamená, že velikost kontejneru je příliš malá.</span><span class="sxs-lookup"><span data-stu-id="56805-126">From our experience, the out of memory exception does not mean the container size is too small.</span></span> <span data-ttu-id="56805-127">Znamená to, že velikost haldy Java (hive.tez.java.opts) je příliš malá.</span><span class="sxs-lookup"><span data-stu-id="56805-127">It means the Java heap size (hive.tez.java.opts) is too small.</span></span> <span data-ttu-id="56805-128">Proto vždy, když se zobrazí nedostatek paměti, můžete zkusit zvýšit **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="56805-128">So whenever you see out of memory, you can try to increase **hive.tez.java.opts**.</span></span> <span data-ttu-id="56805-129">V případě potřeby budete možná muset zvýšit **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="56805-129">If needed you might have to increase **hive.tez.container.size**.</span></span> <span data-ttu-id="56805-130">**Java.opts** nastavení by mělo být přibližně 80 % **container.size**.</span><span class="sxs-lookup"><span data-stu-id="56805-130">The **java.opts** setting should be around 80% of **container.size**.</span></span>

> [!NOTE]
> <span data-ttu-id="56805-131">Nastavení **hive.tez.java.opts** musí být menší než **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="56805-131">The setting **hive.tez.java.opts** must always be smaller than **hive.tez.container.size**.</span></span>
> 
> 

<span data-ttu-id="56805-132">D12 počítač obsahuje 28GB paměti, a proto jsme se rozhodli, přidělte 80 % java.opts pomocí kontejneru velikost 10 GB (10240MB):</span><span class="sxs-lookup"><span data-stu-id="56805-132">Because a D12 machine has 28GB memory, we decided to use a container size of 10GB (10240MB) and assign 80% to java.opts:</span></span>

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

<span data-ttu-id="56805-133">S novým nastavením dotaz byl úspěšně spuštěn v části 10 minut.</span><span class="sxs-lookup"><span data-stu-id="56805-133">With the new settings, the query successfully ran in under 10 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="56805-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56805-134">Next steps</span></span>

<span data-ttu-id="56805-135">Získávání chybu OOM není nutně znamenat, že velikost kontejneru je příliš malá.</span><span class="sxs-lookup"><span data-stu-id="56805-135">Getting an OOM error doesn't necessarily mean the container size is too small.</span></span> <span data-ttu-id="56805-136">Místo toho by měl nakonfigurovat nastavení paměti, aby velikost haldy je vyšší a je alespoň 80 % velikost paměti kontejneru.</span><span class="sxs-lookup"><span data-stu-id="56805-136">Instead, you should configure the memory settings so that the heap size is increased and is at least 80% of the container memory size.</span></span> <span data-ttu-id="56805-137">Optimalizace dotazů Hive, najdete v části [optimalizovat Hive dotazy pro Hadoop v HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span><span class="sxs-lookup"><span data-stu-id="56805-137">For optimizing Hive queries, see [Optimize Hive queries for Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span></span>