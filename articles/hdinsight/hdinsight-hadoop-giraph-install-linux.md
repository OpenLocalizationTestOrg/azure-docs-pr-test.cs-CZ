---
title: "aaaInstall a používat Giraph v HDInsight (Hadoop) - Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall Giraph na HDInsight se systémem Linux clustery pomocí akcí skriptů. Akce skriptu umožní toocustomize hello clusteru během vytváření, změně konfigurace clusteru nebo instalací služby a nástroje."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0f195b65cebf5e24d1808ef33b95b4d362555521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-tooprocess-large-scale-graphs"></a>Nainstalujte Giraph clusterů systému HDInsight Hadoop a používat Giraph tooprocess rozsáhlé grafy

Zjistěte, jak tooinstall Apache Giraph v clusteru HDInsight. Hello funkce akce skriptu HDInsight vám umožní toocustomize clusteru spuštěním skriptu prostředí bash. Skripty lze použít toocustomize clustery během a po vytvoření clusteru.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whatis"></a>Co je Giraph

[Apache Giraph](http://giraph.apache.org/) vám umožní tooperform grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight. Grafy model relace mezi objekty. Například hello připojení mezi směrovači v velkým síťovým typu hello Internet nebo vztahy mezi uživateli v sociálních sítích. Graf zpracování vám umožní tooreason o hello vztahy mezi objekty v grafu, jako například:

* Identifikace potenciální přátel na základě vaší aktuální relací.

* Identifikace hello nejkratší trasy mezi dvěma počítači v síti.

* Výpočet pořadí stránku hello webové stránky.

> [!WARNING]
> Součásti, které jsou součástí clusteru HDInsight hello plně podporované – Microsoft Support pomáhá tooisolate a vyřešit problémy související toothese součásti.
>
> Vlastní komponenty, například Giraph, přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello. Microsoft Support může být schopný tooresolving hello problém. Pokud ne, musíte obrátit komunit s otevřeným zdrojem, kterých se nachází hluboké znalosti pro tuto technologii. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).


## <a name="what-hello-script-does"></a>Jaké skript hello

Tento skript provede hello následující akce:

* Nainstaluje Giraph příliš`/usr/hdp/current/giraph`

* Kopie hello `giraph-examples.jar` toodefault úložiště (WASB) pro váš cluster souborů:`/example/jars/giraph-examples.jar`

## <a name="install"></a>Nainstalujte Giraph pomocí akcí skriptů

Tooinstall skriptu ukázka Giraph v clusteru HDInsight je k dispozici na hello následující umístění:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Tato část obsahuje pokyny jak toouse hello ukázkový skript při vytváření clusteru hello pomocí hello portálu Azure.

> [!NOTE]
> Akce skriptu můžete použít některé z následujících metod hello používá:
> * Azure PowerShell
> * Hello rozhraní příkazového řádku Azure
> * Hello HDInsight .NET SDK
> * Šablony Azure Resource Manageru
> 
> Můžete také použít skript akce tooalready spuštěné clustery. Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).

1. Zahájení vytváření clusteru pomocí kroků hello v [clustery HDInsight se systémem Linux vytvořit](hdinsight-hadoop-create-linux-clusters-portal.md), ale nedokončila vytvoření.

2. Na hello **volitelné konfiguraci** vyberte **akcí skriptů**a zadejte hello následující informace:

   * **NÁZEV**: Zadejte popisný název akce skriptu hello.

   * **Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: Zkontrolujte tuto položku

   * **PRACOVNÍ**: nechte zaškrtnuté políčko tuto položku

   * **ZOOKEEPER**: nechte zaškrtnuté políčko tuto položku

   * **Parametry**: to pole ponechat prázdné

3. Na konci hello hello **akcí skriptů**, použijte hello **vyberte** tlačítko toosave hello konfigurace. Nakonec použijte hello **vyberte** tlačítko dole hello hello **volitelné konfiguraci** okno toosave hello volitelné konfiguraci informace.

4. Pokračovat ve vytváření clusteru hello, jak je popsáno v [clustery HDInsight se systémem Linux vytvořit](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Jak používat Giraph v prostředí HDInsight?

Po vytvoření clusteru hello, použijte následující postup toorun hello SimpleShortestPathsComputation ukázka součástí Giraph hello. Tento příklad používá hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementace pro hledání hello nejkratší cesta mezi objekty v grafu.

1. Připojte toohello clusteru HDInsight pomocí protokolu SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použití hello následující příkaz toocreate soubor s názvem **tiny_graph.txt**:

    ```bash
    nano tiny_graph.txt
    ```

    Použijte hello následující text jako hello obsah tohoto souboru:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Tato data popisuje vztah mezi objekty v řízené grafu, ve formátu hello `[source_id, source_value,[[dest_id], [edge_value],...]]`. Každý řádek představuje vztah mezi `source_id` objektů a jeden nebo více `dest_id` objekty. Hello `edge_value` si lze představit jako hello sílu nebo vzdálenost hello připojení mezi `source_id` a `dest\_id`.

    Vykreslovat limitu, a pomocí hodnoty hello (nebo váhy) jako hello vzdálenost mezi objekty, hello dat může vypadat například hello následující diagram:

    ![tiny_graph.txt vykreslovat jako kroužky řádků různých vzdálenost mezi](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. toosave hello soubor, použijte **Ctrl + X**, pak **Y**a v neposlední řadě **Enter** název souboru tooaccept hello.

4. Použijte následující toostore hello data do primárního úložiště pro váš cluster HDInsight hello:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Spusťte hello SimpleShortestPathsComputation příklad použití hello následující příkaz:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    Hello parametry tohoto příkazu jsou popsané v následující tabulce hello:

   | Parametr | Výsledek |
   | --- | --- |
   | `jar` |Hello soubor jar obsahující příklady hello. |
   | `org.apache.giraph.GiraphRunner` |Třída Hello používá toostart hello příklady. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |Hello příklad, který se používá. V tomto příkladu vypočítá hello nejkratší cestě mezi ID 1 a všechna ID v grafu hello. |
   | `-ca mapred.job.tracker` |Hello headnode pro hello cluster. |
   | `-vif` |Hello toouse vstupní formát pro vstupní data hello. |
   | `-vip` |Hello vstupní datový soubor. |
   | `-vof` |výstupní formát Hello. V tomto příkladu, ID a hodnotu jako prostý text. |
   | `-op` |umístění výstupu Hello. |
   | `-w 2` |Hello počet toouse pracovních procesů. V tomto příkladu 2. |

    Další informace o těchto a dalších parametrů použít s Giraph ukázky najdete v tématu hello [rychlý start Giraph](http://giraph.apache.org/quick_start.html).

6. Po dokončení úlohy hello hello výsledky ukládat do hello **/example/out/shotestpaths** adresáře. Hello názvy výstupního souboru začínat **část-m -** a končit číslo určující hello první, druhý, soubor atd. Použijte následující příkaz tooview hello výstup hello:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    výstup Hello by měla vypadat podobně jako toohello následující text:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Příklad SimpleShortestPathComputation Hello je naprogramováno toostart s objektem ID 1 a najít hello nejkratší cesta tooother objekty. výstup Hello je ve formátu hello `destination_id` a `distance`. Hello `distance` je hello hodnotu (nebo váhy) hello okrajů kterou urazit mezi objektem ID 1 a ID hello cíl.

    Vizualizace tato data, můžete ověřit výsledky hello cestě hello nejkratší cest mezi ID 1 a všechny ostatní objekty. Cesta co nejkratší Hello ID 1 až ID 4 je 5. Tato hodnota je hello celkový vzdálenost mezi <span style="color:orange">ID 1 a 3</span>a potom <span style="color:red">ID 3 a 4</span>.

    ![Kreslení objektů jako kroužky s nejkratší cest mezi](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Další kroky

* [Nainstalovat a používat Hue v clusterech HDInsight](hdinsight-hadoop-hue-linux.md).

* [Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md).
