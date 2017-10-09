---
title: aaaOptimize Hive dotazuje v Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toooptimize vaše Hive dotazuje na Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2016
ms.author: jgao
ms.openlocfilehash: d27f8100e1e9f4823040ff9f693e7b78d6192c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>Optimalizace dotazů Hive v Azure HDInsight

Ve výchozím nastavení nejsou clusterů systému Hadoop optimalizované pro výkon. Tento článek se zabývá některé nejběžnější Hive optimalizace metody údajů o výkonu, můžete použít tooyour dotazy.

## <a name="scale-out-worker-nodes"></a>Horizontální navýšení kapacity uzlů pracovního procesu

Zvýšení hello počet uzlů pracovního procesu v clusteru, můžete využít další mappers a přechodky toobe souběžně. Existují dva způsoby, jak můžete zvýšit možností horizontálního rozšíření kapacity v HDInsight:

* Během hello zřizování můžete zadat hello počet uzlů pracovního procesu pomocí hello portálu Azure, Azure PowerShell nebo rozhraní příkazového řádku pro různé platformy.  Další informace najdete v tématu [Vytvoření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Hello následující snímek obrazovky ukazuje hello pracovní konfigurace uzlu na hello portálu Azure:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* V hello běh můžete taky škálovat cluster bez nutnosti opětovného vytvoření jeden:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Další informace o hello různé virtuální počítače podporované v prostředí HDInsight najdete v tématu [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Povolení Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) je modul alternativní provádění modul toohello MapReduce:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez je rychlejší, protože:

* **Spuštění směrované Acyklické grafu (DAG) jako jednu úlohu v modulu MapReduce hello**. Hello DAG vyžaduje každou sadu mappers toobe následuje jednu sadu přechodky. To způsobí, že více toobe úlohy MapReduce spuštěné pro každý dotaz Hive. Tez nemá takové omezení a může zpracovat komplexní DAG jako jednu úlohu, čímž dojde k minimalizaci režie spuštění úlohy.
* **Zabraňuje zbytečným zápisy**. Z důvodu toomultiple úlohy se spouštějí pro hello stejný dotaz Hive v hello MapReduce stroje, výstup hello Každá úloha se zapíše tooHDFS pro mezilehlá data. Vzhledem k tomu, že minimalizuje počet úloh pro každý dotaz Hive Tez je možné tooavoid nepotřebné zápisu.
* **Minimalizuje zpoždění spuštění**. Tez je lepší zpoždění spuštění možné toominimize snížením hello počet mappers, které potřebuje toostart a také vylepšení optimalizace v průběhu.
* **Opětovně používá kontejnery**. Vždy, když možné Tez je možné tooreuse kontejnery tooensure této latence kvůli toostarting až kontejnery se snižuje.
* **Techniky průběžné optimalizace**. Optimalizace tradičně bylo provedeno během fáze kompilace. Ale je k dispozici další informace o hello vstupy, které umožňují lepší optimalizace za běhu. Tez používá průběžné optimalizace technik, které umožňuje toooptimize hello Další plán do fáze runtime hello.

Další podrobnosti o těchto pojmech najdete v tématu [Apache TEZ](http://hortonworks.com/hadoop/tez/).

Můžete použít jakýkoli dotaz Hive Tez povoleno pomocí prefixu hello dotazu s nastavením hello níže:

    set hive.execution.engine=tez;

Clustery HDInsight se systémem Linux mít Tez ve výchozím nastavení povolené.


## <a name="hive-partitioning"></a>Hive, vytváření oddílů

Vstupně-výstupní operace je hello potíže hlavní výkonu pro spouštění dotazů Hive. je možné zlepšit výkon Hello, pokud hello množství dat, který potřebuje toobe přečíst, může být nižší. Ve výchozím nastavení kontrolu dotazů Hive celých tabulek Hive. Toto je skvělá pro dotazy jako prohledávání tabulky. Ale pro dotazy, které stačí tooscan malé množství dat (například dotazy s filtrování), toto chování vytvoří nepotřebné režie. Vytváření oddílů Hive umožňuje Hive dotazy tooaccess pouze hello nezbytné množství dat v tabulek Hive.

Vytváření oddílů Hive je implementováno modulem reorganizace hello nezpracovaná data do nového adresáře s každý oddíl má svou vlastní directory – kde je definován hello oddílu uživatelem hello. Hello následující diagram znázorňuje dělení tabulku Hive podle sloupce hello *roku*. Pro každý rok se vytvoří nový adresář.

![Dělení na oddíly][image-hdi-optimize-hive-partitioning_1]

Některé rozdělení aspekty:

* **Proveďte není v oddílu** – vytváření oddílů pro sloupce s pouze několik hodnot může způsobit několik oddílů. Například vytváření oddílů na pohlaví pouze vytvoří dva oddíly toobe vytvořili (mužského a ženského), tedy pouze snížit latenci hello nejvýše o polovinu.
* **Proveďte nikoli prostřednictvím oddílu** – na jiné extreme Dobrý den, vytváření oddílů u sloupce s jedinečnou hodnotu (například ID uživatele) způsobí, že více oddílů. V oddílu způsobí, že mnoho přízvuk na hello clusteru namenode jako má toohandle hello velký počet adresářů.
* **Vyhněte se data zkosení** -vyberte klíč rozdělení dobře tak, aby všechny oddíly jsou i velikost. Příklad rozdělení do oddílů na *stavu* může způsobit hello počet záznamů v části kalifornské toobe téměř 30 x u Vermont kvůli toohello rozdíl v naplnění.

toocreate tabulku oddílů použijte hello *rozdělena na oddíly pomocí* klauzule:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Po vytvoření hello dělenou tabulku můžete buď vytvořit statické dělení nebo dynamické rozdělení.

* **Statické dělení** znamená, že máte již horizontálně dělená data v hello příslušným adresáře a můžete požádat Hive oddíly ručně podle umístění adresáře hello. Následující fragment kódu Hello je příklad.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dynamické vytváření oddílů** znamená, že oddíly toocreate Hive automaticky za vás. Vzhledem k tomu, že jsme vytvořili již hello vytváření oddílů tabulky z hello pracovní tabulky, všechny potřebujeme toodo je tabulka toohello rozdělena na oddíly vložení dat:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Další podrobnosti najdete v tématu [rozdělena na oddíly tabulky](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-hello-orcfile-format"></a>Použijte formát ORCFile hello
Hive podporuje jiné formáty souborů. Například:

* **Text**: Toto je výchozí formát souboru hello a funguje s většinu scénářů
* **Avro**: funguje dobře pro scénáře interoperability
* **ORC/Parquet**: nejvhodnější pro výkon

Formát ORC (optimalizované řádek sloupcovém) je vysoce efektivní způsob toostore Hive data. Porovnání tooother formátů, ORC má hello následující výhody:

* Podpora pro komplexní typy, včetně data a času a částečně strukturovaných a komplexní typy
* až too70 % komprese
* indexy každých 10 000 řádky, které povolit přeskočení řádků
* významné pokles v běhu provádění

Formát ORC tooenable, nejprve vytvoříte tabulku s klauzulí hello *uložené jako ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

V dalším kroku vložení dat toohello ORC tabulky z hello pracovní tabulky. Například:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
            L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Si můžete přečíst informace o formátu ORC hello [zde](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vectorization

Vectorization umožňuje Hive tooprocess dávky 1024 řádků společně, namísto zpracování jeden řádek v čase. Znamená to, že jednoduché operace jsou rychlejší provést, protože menší interní kód potřebuje toorun.

tooenable vectorization předpony dotaz Hive s hello následující nastavení:

    set hive.vectorized.execution.enabled = true;

Další informace najdete v tématu [Vectorized provádění dotazu](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Ostatní metody optimalizace
Existují další metody optimalizace, které můžete zvážit, například:

* **Hive bucketing:** technika, který umožňuje toocluster nebo segment velkého objemu dat výkonu dotazů toooptimize.
* **Připojení k optimalizaci:** optimalizace spouštění dotazů Hive na plánování tooimprove hello efektivitu spojení a snížit hello potřebu pomocné parametry uživatele. Další informace najdete v tématu [připojení optimalizace](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Zvýšit přechodky**.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili několik běžné metody optimalizace dotazů Hive. toolearn více, najdete v části hello následující články:

* [Používání Apache Hive v HDInsight](hdinsight-use-hive.md)
* [Analýza dat zpoždění letu pomocí Hive v HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Analýza dat Twitteru pomocí Hive v HDInsight](hdinsight-analyze-twitter-data.md)
* [Analýza dat snímačů pomocí hello Hive dotaz konzoly systému Hadoop v HDInsight](hdinsight-hive-analyze-sensor-data.md)
* [Použijte Hive s HDInsight tooanalyze protokoly z webů](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
