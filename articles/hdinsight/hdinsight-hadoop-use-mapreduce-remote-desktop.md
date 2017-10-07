---
title: "aaaMapReduce a Vzdálená plocha s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak tooHadoop tooconnect toouse vzdálené plochy na HDInsight a spuštění úloh MapReduce."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: bdbbcf59ca86dd1b170471aa9e12334a04c23667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Používání nástroje MapReduce v Hadoop v HDInsight pomocí vzdálené plochy
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

V tomto článku se dozvíte, jak clusteru tooconnect tooa Hadoop v HDInsight pomocí vzdálené plochy a potom spusťte úloh MapReduce pomocí příkazu Hadoop hello.

> [!IMPORTANT]
> Vzdálená plocha je dostupná pouze na clustery HDInsight se systémem Windows. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 nebo větší, projděte si téma [používání MapReduce s SSH](hdinsight-hadoop-use-mapreduce-ssh.md) informace o připojení toohello HDInsight cluster a spuštění úloh MapReduce.

## <a id="prereq"></a>Požadavky
toocomplete hello kroky v tomto článku, budete potřebovat následující hello:

* Cluster HDInsight se systémem Windows (Hadoop v HDInsight)
* Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7

## <a id="connect"></a>Připojit pomocí vzdálené plochy
Povolení vzdálené plochy pro hello HDInsight cluster a pak připojit tooit podle následujících pokynů hello [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Použijte příkaz Hadoop hello
Pokud jste desktop připojený toohello pro hello HDInsight cluster, použijte následující kroky toorun úlohu MapReduce pomocí příkazu Hadoop hello hello:

1. Z plochy hello HDInsight, spusťte hello **Hadoop příkazového řádku**. Otevře se nové příkazový řádek v hello **c:\apps\dist\hadoop-&lt;číslo verze >** adresáře.

   > [!NOTE]
   > číslo verze Hello změní, když se aktualizuje Hadoop. Hello **HADOOP_HOME** proměnnou prostředí může být cesta použité toofind hello. Například `cd %HADOOP_HOME%` změny adresáře toohello Hadoop adresáři, aniž byste si číslo verze tooknow hello.
   >
   >
2. toouse hello **Hadoop** příkaz toorun úlohu MapReduce příklad, použijte následující příkaz hello:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Tím se spustí hello **wordcount** třídy, která je součástí hello **hadoop-mapreduce-examples.jar** soubor v aktuálním adresáři hello. Jako vstup, používá hello **wasb://example/data/gutenberg/davinci.txt** dokumentu a výstup je uložený v: **wasb: / / / Příklad/data/WordCountOutput**.

   > [!NOTE]
   > Další informace týkající se těchto dat. MapReduce úlohy a hello příklad najdete v tématu <a href="hdinsight-use-mapreduce.md">použití MapReduce v HDInsight Hadoop</a>.
   >
   >
3. Úloha Hello vysílá podrobnosti, jako je zpracován a vrátí informace podobné toohello následující po dokončení úlohy hello:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Po dokončení úlohy hello, použijte následující příkaz toolist hello výstupní soubory uložené v hello **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    To by měl zobrazit dva soubory, **_SUCCESS** a **část r-00000**. Hello **část r-00000** soubor obsahuje hello výstup pro tuto úlohu.

   > [!NOTE]
   > Některé úlohy MapReduce může rozdělit do několika hello výsledky **část r-###** soubory. Pokud ano, použít hello ### přípona tooindicate hello pořadí souborů hello.
   >
   >
5. tooview hello výstup hello použijte následující příkaz:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Zobrazí se seznam hello slova, které jsou obsaženy v hello **wasb://example/data/gutenberg/davinci.txt** souboru, společně s hello počet jednotlivých slov došlo k chybě. Hello tady je příklad hello dat, který bude obsažen v souboru hello:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Shrnutí
Jak vidíte, bude hello příkaz Hadoop na clusteru HDInsight a pak výstup úlohy hello zobrazení poskytuje snadný způsob toorun úloh MapReduce.

## <a id="nextsteps"></a>Další kroky
Obecné informace o úloh MapReduce v HDInsight:

* [Používání nástroje MapReduce systému HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
