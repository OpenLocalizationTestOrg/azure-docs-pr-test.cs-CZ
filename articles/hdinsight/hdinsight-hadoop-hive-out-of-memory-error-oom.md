---
title: "aaaFix Hive nedostatek paměti v Azure HDInsight | Microsoft Docs"
description: "Opravte Hive nedostatek paměti v HDInsight. scénář zákazníka Hello je dotaz napříč mnoha velké tabulky."
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
ms.openlocfilehash: 00a12969322c1e74434ba6593ffd098f342edd84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a><span data-ttu-id="173e8-105">Opravte Hive nedostatek paměti v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="173e8-105">Fix a Hive out of memory error in Azure HDInsight</span></span>

<span data-ttu-id="173e8-106">Zjistěte, jak toofix Hive nedostatek paměti při zpracování velkých tabulky nakonfigurováním nastavení paměti Hive.</span><span class="sxs-lookup"><span data-stu-id="173e8-106">Learn how toofix a Hive out of memory error when process large tables by configuring Hive memory settings.</span></span>

## <a name="run-hive-query-against-large-tables"></a><span data-ttu-id="173e8-107">Spusťte dotaz Hive na velké tabulky</span><span class="sxs-lookup"><span data-stu-id="173e8-107">Run Hive query against large tables</span></span>

<span data-ttu-id="173e8-108">Zákazník spustili dotazů Hive:</span><span class="sxs-lookup"><span data-stu-id="173e8-108">A customer ran a Hive query:</span></span>

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

<span data-ttu-id="173e8-109">Některé drobné odlišnosti tohoto dotazu:</span><span class="sxs-lookup"><span data-stu-id="173e8-109">Some nuances of this query:</span></span>

* <span data-ttu-id="173e8-110">T1 je alias tooa big tabulka, tabulky1, který má spoustu typy sloupců řetězec.</span><span class="sxs-lookup"><span data-stu-id="173e8-110">T1 is an alias tooa big table, TABLE1, which has lots of STRING column types.</span></span>
* <span data-ttu-id="173e8-111">Ostatní tabulky není, jsou velký, ale máte mnoho sloupců.</span><span class="sxs-lookup"><span data-stu-id="173e8-111">Other tables are not that big but do have many columns.</span></span>
* <span data-ttu-id="173e8-112">Všechny tabulky jsou propojení mezi sebou, v některých případech s více sloupců v tabulky1 a ostatní.</span><span class="sxs-lookup"><span data-stu-id="173e8-112">All tables are joining each other, in some cases with multiple columns in TABLE1 and others.</span></span>

<span data-ttu-id="173e8-113">dotaz Hive Hello trvalo toofinish 26 minut na clusteru A3 HDInsight 24 uzlu.</span><span class="sxs-lookup"><span data-stu-id="173e8-113">hello Hive query took 26 minutes toofinish on a 24 node A3 HDInsight cluster.</span></span> <span data-ttu-id="173e8-114">Hello zákazníka upozornit hello následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="173e8-114">hello customer noticed hello following warning messages:</span></span>

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

<span data-ttu-id="173e8-115">Pomocí modulu Tez hello.</span><span class="sxs-lookup"><span data-stu-id="173e8-115">By using hello Tez execution engine.</span></span> <span data-ttu-id="173e8-116">Hello stejný dotaz spustili 15 minut a pak vrátil hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="173e8-116">hello same query ran for 15 minutes, and then threw hello following error:</span></span>

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

<span data-ttu-id="173e8-117">Chyba Hello zůstává při použití větší virtuální počítač (například D12).</span><span class="sxs-lookup"><span data-stu-id="173e8-117">hello error remains when using a bigger virtual machine (for example, D12).</span></span>


## <a name="debug-hello-out-of-memory-error"></a><span data-ttu-id="173e8-118">Ladění hello nedostatek paměti</span><span class="sxs-lookup"><span data-stu-id="173e8-118">Debug hello out of memory error</span></span>

<span data-ttu-id="173e8-119">Jedním z problémů hello způsobuje hello nedostatek paměti bylo nalezeno naše podporu a vývojové týmy společně [známý problém popsaný v hello Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span><span class="sxs-lookup"><span data-stu-id="173e8-119">Our support and engineering teams together found one of hello issues causing hello out of memory error was a [known issue described in hello Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span></span>

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if hello sum  of tables sizes in hello map join is less than noconditionaltask.size hello plan would generate a Map join, hello issue with this is that hello calculation doesnt take into account hello overhead introduced by different HashTable implementation as results if hello sum of input sizes is smaller than hello noconditionaltask size by a small margin queries will hit OOM.

<span data-ttu-id="173e8-120">Hello **hive.auto.convert.join.noconditionaltask** v hello hive-site.xml soubor byl nastaven příliš**true**:</span><span class="sxs-lookup"><span data-stu-id="173e8-120">hello **hive.auto.convert.join.noconditionaltask** in hello hive-site.xml file was set too**true**:</span></span>

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables hello optimization about converting common join into mapjoin based on hello input file size.
              If this parameter is on, and hello sum of size for n-1 of hello tables/partitions for a n-way join is smaller than the
              specified size, hello join is directly converted tooa mapjoin (there is no conditional task).
        </description>
      </property>

<span data-ttu-id="173e8-121">Je pravděpodobné, spojení mapa byla hello příčinu hello Java haldy místo naše chyby paměti.</span><span class="sxs-lookup"><span data-stu-id="173e8-121">It is likely map join was hello cause of hello Java Heap Space our of memory error.</span></span> <span data-ttu-id="173e8-122">Jak je popsáno v příspěvku blogu hello [nastavení paměti Hadoop Yarn v HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), když modul pro spuštění je použité hello haldy místo využité Tez ve skutečnosti patří toohello Tez kontejneru.</span><span class="sxs-lookup"><span data-stu-id="173e8-122">As explained in hello blog post [Hadoop Yarn memory settings in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), when Tez execution engine is used hello heap space used actually belongs toohello Tez container.</span></span> <span data-ttu-id="173e8-123">Viz následující obrázek popisující hello Tez kontejneru paměti hello.</span><span class="sxs-lookup"><span data-stu-id="173e8-123">See hello following image describing hello Tez container memory.</span></span>

![Diagram paměti kontejneru tez: Hive nedostatek paměti](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

<span data-ttu-id="173e8-125">Jak hello příspěvku na blogu naznačuje, hello následující dvě nastavení paměti definování hello kontejneru paměti haldy hello: **hive.tez.container.size** a **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="173e8-125">As hello blog post suggests, hello following two memory settings define hello container memory for hello heap: **hive.tez.container.size** and **hive.tez.java.opts**.</span></span> <span data-ttu-id="173e8-126">Z našich zkušeností hello nedostatku paměti, neznamená, že velikost kontejneru hello je příliš malá.</span><span class="sxs-lookup"><span data-stu-id="173e8-126">From our experience, hello out of memory exception does not mean hello container size is too small.</span></span> <span data-ttu-id="173e8-127">Znamená to, že hello velikost haldy Java (hive.tez.java.opts) je příliš malá.</span><span class="sxs-lookup"><span data-stu-id="173e8-127">It means hello Java heap size (hive.tez.java.opts) is too small.</span></span> <span data-ttu-id="173e8-128">Proto vždy, když se zobrazí nedostatek paměti, můžete zkusit tooincrease **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="173e8-128">So whenever you see out of memory, you can try tooincrease **hive.tez.java.opts**.</span></span> <span data-ttu-id="173e8-129">V případě potřeby můžete mít tooincrease **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="173e8-129">If needed you might have tooincrease **hive.tez.container.size**.</span></span> <span data-ttu-id="173e8-130">Hello **java.opts** nastavení by mělo být přibližně 80 % **container.size**.</span><span class="sxs-lookup"><span data-stu-id="173e8-130">hello **java.opts** setting should be around 80% of **container.size**.</span></span>

> [!NOTE]
> <span data-ttu-id="173e8-131">nastavení Hello **hive.tez.java.opts** musí být menší než **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="173e8-131">hello setting **hive.tez.java.opts** must always be smaller than **hive.tez.container.size**.</span></span>
> 
> 

<span data-ttu-id="173e8-132">D12 počítač obsahuje 28GB paměti, a proto jsme se rozhodli toouse kontejneru velikost 10 GB (10240MB) a přiřaďte 80 % toojava.opts:</span><span class="sxs-lookup"><span data-stu-id="173e8-132">Because a D12 machine has 28GB memory, we decided toouse a container size of 10GB (10240MB) and assign 80% toojava.opts:</span></span>

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

<span data-ttu-id="173e8-133">Nové nastavení hello hello dotazu úspěšně spustil v části 10 minut.</span><span class="sxs-lookup"><span data-stu-id="173e8-133">With hello new settings, hello query successfully ran in under 10 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="173e8-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="173e8-134">Next steps</span></span>

<span data-ttu-id="173e8-135">Získávání chybu OOM není nutně znamenat, že velikost kontejneru hello je příliš malá.</span><span class="sxs-lookup"><span data-stu-id="173e8-135">Getting an OOM error doesn't necessarily mean hello container size is too small.</span></span> <span data-ttu-id="173e8-136">Místo toho byste měli nakonfigurovat nastavení paměti hello tak, aby velikost haldy hello je vyšší a je alespoň 80 % velikosti paměti hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="173e8-136">Instead, you should configure hello memory settings so that hello heap size is increased and is at least 80% of hello container memory size.</span></span> <span data-ttu-id="173e8-137">Optimalizace dotazů Hive, najdete v části [optimalizovat Hive dotazy pro Hadoop v HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span><span class="sxs-lookup"><span data-stu-id="173e8-137">For optimizing Hive queries, see [Optimize Hive queries for Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span></span>
