---
title: "aaaMapReduce a připojení SSH s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse SSH toorun MapReduce úlohy pomocí Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 9626577687fc5cc119a39d65a9c45298f57f81c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Používání nástroje MapReduce s Hadoop v HDInsight pomocí protokolu SSH

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Zjistěte, jak toosubmit MapReduce úlohy z tooHDInsight připojení Secure Shell (SSH).

> [!NOTE]
> Pokud jste již obeznámeni s používáním systémem Linux Hadoop servery, ale jsou nové tooHDInsight najdete [HDInsight se systémem Linux tipy](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Požadavky

* Cluster HDInsight se systémem Linux (Hadoop v HDInsight)

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Klientem SSH. Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>Připojení pomocí protokolu SSH

Připojte toohello clusteru pomocí protokolu SSH. Například následující příkaz hello připojuje tooa clusteru s názvem **myhdinsight**:

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

**Pokud používáte klíč certifikátu pro ověřování SSH**, budete pravděpodobně toospecify hello umístění hello privátní klíč klientského systému, například:

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

**Pokud použijete heslo pro ověřování SSH**, je nutné heslo hello tooprovide po zobrazení výzvy.

Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Použít příkazy Hadoop

1. Jakmile se připojené toohello clusteru HDInsight pomocí hello následující příkaz toostart úlohu MapReduce:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Tento příkaz spustí hello `wordcount` třídy, která je součástí hello `hadoop-mapreduce-examples.jar` souboru. Používá hello `/example/data/gutenberg/davinci.txt` dokumentu jako vstup a výstup je uloženo na `/example/data/WordCountOutput`.

    > [!NOTE]
    > Další informace týkající se těchto dat. MapReduce úlohy a hello příklad najdete v tématu [použití MapReduce v Hadoop v HDInsight](hdinsight-use-mapreduce.md).

2. Úloha Hello vysílá podrobnosti zpracovává a vrátí informace podobné toohello po dokončení úlohy hello následující text:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Po dokončení úlohy hello, použijte následující příkaz toolist hello výstupní soubory hello:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Tento příkaz zobrazí dva soubory, `_SUCCESS` a `part-r-00000`. Hello `part-r-00000` soubor obsahuje hello výstup pro tuto úlohu.

    > [!NOTE]
    > Některé úlohy MapReduce může rozdělit do několika hello výsledky **část r-###** soubory. Pokud ano, použít hello ### přípona tooindicate hello pořadí souborů hello.

4. tooview hello výstup hello použijte následující příkaz:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Tento příkaz zobrazí seznam hello slova, které jsou obsaženy v hello **wasb://example/data/gutenberg/davinci.txt** souborové služby a hello stanovený počet jednotlivých slov došlo k chybě. Hello následující text je příkladem hello data obsažená v souboru hello:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Shrnutí

Jak můžete vidět, poskytují příkazy Hadoop toorun snadný způsob úloh MapReduce do clusteru HDInsight a pak výstup úlohy hello zobrazení.

## <a id="nextsteps"></a>Další kroky

Obecné informace o úloh MapReduce v HDInsight:

* [Používání nástroje MapReduce systému HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
