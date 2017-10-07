---
title: "aaaTroubleshoot YARN pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi toocommon dotazy týkající se práce s Apache Hadoop YARN a Azure HDInsight."
keywords: "Azure HDInsight, YARN – nejčastější dotazy, řešení potíží s průvodce, časté otázky"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: 800d9738cb27e05a64db470ee58565af3b85aa99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a>Řešení potíží YARN pomocí Azure HDInsight

Další informace o hello nejčastější problémy a jejich řešení při práci s Apache Hadoop YARN datové části v Apache Ambari.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Jak vytvořit novou frontu YARN v clusteru


### <a name="resolution-steps"></a>Kroky řešení 

Použití hello následující kroky v Ambari toocreate novou frontu YARN a pak vyvážit přidělení kapacity hello mezi všechny fronty hello. 

V tomto příkladu dvě existující fronty (**výchozí** a **thriftsvr**) i se změnilo z 50 % kapacity too25 % kapacity, která umožňuje hello nové fronty (spark) 50 % kapacity.
| Fronta | Kapacita | Maximální kapacita |
| --- | --- | --- | --- |
| Výchozí | 25 % | 50% |
| thrftsvr | 25 % | 50% |
| Spark | 50% | 50% |

1. Vyberte hello **zobrazení Ambari** ikonu a vyberte hello vzor mřížky. Potom vyberte **správce front YARN**.

    ![Vyberte ikonu pro zobrazení Ambari hello](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. Vyberte hello **výchozí** fronty.

    ![Vyberte výchozí fronty hello](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. Pro hello **výchozí** fronty, změňte hello **kapacity** z 50 % too25 %. Pro hello **thriftsvr** fronty, změňte hello **kapacity** too25 %.

    ![Změna hello kapacitu too25 % pro hello výchozí a thriftsvr fronty](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. Vyberte toocreate novou frontu, **přidat fronty**.

    ![Vyberte Přidat fronty](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. Název hello novou frontu.

    ![Název fronty hello Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. Nechte hello **kapacity** hodnoty na 50 % a pak vyberte hello **akce** tlačítko.

    ![Kliknutím na tlačítko akce hello](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. Vyberte **uložte a aktualizujte fronty**.

    ![Vyberte uložte a aktualizujte fronty](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

Tyto změny se projeví okamžitě na hello uživatelském rozhraní YARN plánovače.

### <a name="additional-reading"></a>Další čtení

- [YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Jak lze stáhnout protokoly YARN z clusteru


### <a name="resolution-steps"></a>Kroky řešení 

1. Připojte toohello clusteru HDInsight pomocí klienta Secure Shell (SSH). Další informace najdete v tématu [další čtení](#additional-reading-2).

2. toolist všechny hello ID aplikace hello YARN aplikací, které jsou aktuálně spuštěny, spusťte následující příkaz hello:

    ```apache
    yarn top
    ```
    Hello ID jsou uvedeny v hello **APPLICATIONID** sloupce. Protokoly můžete stáhnout z hello **APPLICATIONID** sloupce.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. kontejner protokoly YARN toodownload pro všechny servery aplikace, použijte následující příkaz hello:
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Tento příkaz vytvoří soubor protokolu s názvem amlogs.txt. 

4. kontejner protokoly YARN toodownload pro pouze hello nejnovější aplikace hlavní server, použijte následující příkaz hello:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Tento příkaz vytvoří soubor protokolu s názvem latestamlogs.txt. 

4. kontejner protokoly YARN toodownload pro hello první dvě aplikace hlavních serverů, použijte následující příkaz hello:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    Tento příkaz vytvoří soubor protokolu s názvem first2amlogs.txt. 

5. použít všechny protokoly YARN kontejneru, toodownload hello následující příkaz:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Tento příkaz vytvoří soubor protokolu s názvem logs.txt. 

6. toodownload hello kontejneru protokolu YARN pro specifický kontejner, hello použijte následující příkaz:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    Tento příkaz vytvoří soubor protokolu s názvem containerlogs.txt.

### <a name="additional-reading-2"></a>Další čtení

- [Připojit tooHDInsight (Hadoop) pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN koncepty a aplikací](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







