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
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a>Řešení potíží Hive pomocí Azure HDInsight

Další informace o hello nejvyšší otázky a jejich řešení při práci s Apache Hive datové části v Apache Ambari.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Jak exportovat metaúložiště Hive a importovat na jiný cluster


### <a name="resolution-steps"></a>Kroky řešení

1. Připojte toohello clusteru HDInsight pomocí klienta Secure Shell (SSH). Další informace najdete v tématu [další čtení](#additional-reading-end).

2. Spusťte následující příkaz na clusteru HDInsight hello, ze kterého mají být tooexport hello metaúložiště hello:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  Tento příkaz vytvoří soubor s názvem allatables.sql.

3. Zkopírujte hello souboru alltables.sql toohello nový cluster HDInsight a pak spusťte následující příkaz hello:

  ```apache
  hive -f alltables.sql
  ```

Hello kód v kroky řešení hello předpokládá, že data, která jsou cesty na novém clusteru hello stejné hello jako hello cesty k datům v původním clusteru hello. Pokud cesty k datům hello liší, můžete ručně upravit hello generované alltables.sql souboru tooreflect žádné změny.

### <a name="additional-reading"></a>Další čtení

- [Připojit tooan clusteru HDInsight pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Jak najít protokoly Hive v clusteru

### <a name="resolution-steps"></a>Kroky řešení

1. Připojte toohello clusteru HDInsight pomocí SSH. Další informace najdete v tématu **další čtení**.

2. protokoly klienta tooview Hive, použijte následující příkaz hello:

  ```apache
  /tmp/<username>/hive.log 
  ```

3. protokoly metaúložiště Hive tooview, použijte následující příkaz hello:

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. tooview Hiveserver protokoly, použijte následující příkaz hello:

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a>Další čtení

- [Připojit tooan clusteru HDInsight pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-hello-hive-shell-with-specific-configurations-on-a-cluster"></a>Jak spuštění hello prostředí Hive s konkrétní konfigurací v clusteru

### <a name="resolution-steps"></a>Kroky řešení

1. Zadejte pár klíč hodnota konfigurace při spuštění hello prostředí Hive. Další informace najdete v tématu [další čtení](#additional-reading-end).

  ```apache
  hive -hiveconf a=b 
  ```

2. toolist všechny efektivní konfigurace v prostředí Hive, hello použijte následující příkaz:

  ```apache
  hive> set;
  ```

  Například použijte následující příkaz prostředí Hive toostart s protokolováním ladění povoleno v konzole hello hello:

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a>Další čtení

- [Vlastnosti konfigurace Hive](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Jak můžu analyzovat data Tez DAG v clusteru kritické cestě


### <a name="resolution-steps"></a>Kroky řešení
 
1. tooanalyze Apache Tez řízené Acyklické grafu (DAG) na clusteru kritické graf, připojit toohello clusteru HDInsight pomocí SSH. Další informace najdete v tématu [další čtení](#additional-reading-end).

2. Na příkazovém řádku spusťte následující příkaz hello:
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. toolist jiné analyzátory, které se dají použít tooanalyze Tez DAG použijte hello následující příkaz:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  Ukázkový program je nutné zadat jako první argument hello.

  Program platné názvy patří:
    - **ContainerReuseAnalyzer**: tisk podrobností o opakované použití kontejneru v DAG
    - **CriticalPath**: Find hello kritické cesta DAG
    - **LocalityAnalyzer**: tisk polohu podrobností v DAG
    - **ShuffleTimeAnalyzer**: analyzovat podrobnosti času náhodně hello v DAG
    - **SkewAnalyzer**: Analýza hello zkosení podrobnosti v DAG
    - **SlowNodeAnalyzer**: tisk podrobností uzlu v DAG
    - **SlowTaskIdentifier**: tisk podrobnosti pomalé úlohy v DAG
    - **SlowestVertexAnalyzer**: tisk nejpomalejší vrchol podrobností v DAG
    - **SpillAnalyzer**: tisk přepadového podrobnosti v DAG
    - **TaskConcurrencyAnalyzer**: tisk souběžnosti podrobnosti úlohy hello v DAG
    - **VertexLevelCriticalPathAnalyzer**: najít hello kritické cesty na úrovni vrchol v DAG


### <a name="additional-reading"></a>Další čtení

- [Připojit tooan clusteru HDInsight pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Jak lze stáhnout Tez DAG data z clusteru


#### <a name="resolution-steps"></a>Kroky řešení

Existují dva způsoby toocollect hello Tez DAG dat:

- Z příkazového řádku hello:
 
    Připojte toohello clusteru HDInsight pomocí SSH. Hello příkazového řádku spusťte následující příkaz hello:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- Použijte hello zobrazení Ambari Tez:
   
  1. Přejděte tooAmbari. 
  2. Zobrazení přejděte tooTez (pod hello ikonu dlaždice v pravém horním rohu hello). 
  3. Vyberte hello chcete tooview DAG.
  4. Vyberte **stahování dat**.

### <a name="additional-reading-end"></a>Další čtení

[Připojit tooan clusteru HDInsight pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md)






