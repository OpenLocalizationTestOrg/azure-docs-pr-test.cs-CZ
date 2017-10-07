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
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a>Opravte Hive nedostatek paměti v Azure HDInsight

Zjistěte, jak toofix Hive nedostatek paměti při zpracování velkých tabulky nakonfigurováním nastavení paměti Hive.

## <a name="run-hive-query-against-large-tables"></a>Spusťte dotaz Hive na velké tabulky

Zákazník spustili dotazů Hive:

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

Některé drobné odlišnosti tohoto dotazu:

* T1 je alias tooa big tabulka, tabulky1, který má spoustu typy sloupců řetězec.
* Ostatní tabulky není, jsou velký, ale máte mnoho sloupců.
* Všechny tabulky jsou propojení mezi sebou, v některých případech s více sloupců v tabulky1 a ostatní.

dotaz Hive Hello trvalo toofinish 26 minut na clusteru A3 HDInsight 24 uzlu. Hello zákazníka upozornit hello následující upozornění:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Pomocí modulu Tez hello. Hello stejný dotaz spustili 15 minut a pak vrátil hello následující chybě:

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

Chyba Hello zůstává při použití větší virtuální počítač (například D12).


## <a name="debug-hello-out-of-memory-error"></a>Ladění hello nedostatek paměti

Jedním z problémů hello způsobuje hello nedostatek paměti bylo nalezeno naše podporu a vývojové týmy společně [známý problém popsaný v hello Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if hello sum  of tables sizes in hello map join is less than noconditionaltask.size hello plan would generate a Map join, hello issue with this is that hello calculation doesnt take into account hello overhead introduced by different HashTable implementation as results if hello sum of input sizes is smaller than hello noconditionaltask size by a small margin queries will hit OOM.

Hello **hive.auto.convert.join.noconditionaltask** v hello hive-site.xml soubor byl nastaven příliš**true**:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables hello optimization about converting common join into mapjoin based on hello input file size.
              If this parameter is on, and hello sum of size for n-1 of hello tables/partitions for a n-way join is smaller than the
              specified size, hello join is directly converted tooa mapjoin (there is no conditional task).
        </description>
      </property>

Je pravděpodobné, spojení mapa byla hello příčinu hello Java haldy místo naše chyby paměti. Jak je popsáno v příspěvku blogu hello [nastavení paměti Hadoop Yarn v HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), když modul pro spuštění je použité hello haldy místo využité Tez ve skutečnosti patří toohello Tez kontejneru. Viz následující obrázek popisující hello Tez kontejneru paměti hello.

![Diagram paměti kontejneru tez: Hive nedostatek paměti](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Jak hello příspěvku na blogu naznačuje, hello následující dvě nastavení paměti definování hello kontejneru paměti haldy hello: **hive.tez.container.size** a **hive.tez.java.opts**. Z našich zkušeností hello nedostatku paměti, neznamená, že velikost kontejneru hello je příliš malá. Znamená to, že hello velikost haldy Java (hive.tez.java.opts) je příliš malá. Proto vždy, když se zobrazí nedostatek paměti, můžete zkusit tooincrease **hive.tez.java.opts**. V případě potřeby můžete mít tooincrease **hive.tez.container.size**. Hello **java.opts** nastavení by mělo být přibližně 80 % **container.size**.

> [!NOTE]
> nastavení Hello **hive.tez.java.opts** musí být menší než **hive.tez.container.size**.
> 
> 

D12 počítač obsahuje 28GB paměti, a proto jsme se rozhodli toouse kontejneru velikost 10 GB (10240MB) a přiřaďte 80 % toojava.opts:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Nové nastavení hello hello dotazu úspěšně spustil v části 10 minut.

## <a name="next-steps"></a>Další kroky

Získávání chybu OOM není nutně znamenat, že velikost kontejneru hello je příliš malá. Místo toho byste měli nakonfigurovat nastavení paměti hello tak, aby velikost haldy hello je vyšší a je alespoň 80 % velikosti paměti hello kontejneru. Optimalizace dotazů Hive, najdete v části [optimalizovat Hive dotazy pro Hadoop v HDInsight](hdinsight-hadoop-optimize-hive-query.md).
