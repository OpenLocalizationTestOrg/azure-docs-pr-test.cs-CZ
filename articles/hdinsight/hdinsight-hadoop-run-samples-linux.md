---
title: "Příklady aaaRun MapReduce s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Začněte používat MapReduce ukázky v jar souborů zahrnutých v HDInsight. Použití SSH tooconnect toohello clusteru a pak použít hello Hadoop příkaz toorun ukázkové úlohy."
keywords: "Příklad jar hadoop, příklady jar hadoop, příklady mapreduce hadoop, mapreduce příklady"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1d2a0b9-1659-4fab-921e-4a8990cbb30a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 7a16bbd51eb17570fcaa3b1e0f5990fa889c106a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-mapreduce-examples-included-in-hdinsight"></a>Spuštění příkladů MapReduce hello zahrnuté v HDInsight

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Zjistěte, jak toorun hello MapReduce příklady součástí Hadoop v HDInsight.

## <a name="prerequisites"></a>Požadavky

* **Cluster služby HDInsight**: najdete v části [Začínáme pomocí Hadoop Hive v HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]
    > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Klientem SSH**: Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="hello-mapreduce-examples"></a>Příklady MapReduce Hello

**Umístění**: hello ukázky jsou umístěny na hello cluster HDInsight na `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**Obsah**: hello následující ukázky jsou obsaženy v archivu:

* `aggregatewordcount`: Agregace na základě mapreduce program, který udává hello slova v hello vstupní soubory.
* `aggregatewordhist`: Agregace na základě mapreduce program, který vypočítá hello histogramu hello slova v hello vstupní soubory.
* `bbp`: Mapreduce program, který používá Baileyův. Borwein Plouffe toocompute přesný číslic čísla pí.
* `dbcount`: Příklad úlohu, která udává hello Stránkové zobrazení protokolů uložených v databázi.
* `distbbp`: Mapreduce program, který používá přesný bits BBP typ vzorce toocompute pí.
* `grep`: Mapreduce program, který udává hello odpovídá z regex ve vstupu hello.
* `join`: Úlohu, která provede spojení přes seřadit, rovnoměrně rozděleny do oddílů datových sad.
* `multifilewc`: Úlohu, která počty slov z několika souborů.
* `pentomino`: Dlaždici mapreduce, kterým program toofind řešení toopentomino problémy.
* `pi`: Mapreduce program, který odhadne pí pomocí jako Monte Carlo metoda.
* `randomtextwriter`: Mapreduce program, který zapíše 10 GB náhodné textový dat na jeden uzel.
* `randomwriter`: Mapreduce program, který zapíše 10 GB náhodná data na jeden uzel.
* `secondarysort`: Příklad definování sekundární řazení toohello snížit fáze.
* `sort`: Mapreduce program, který seřadí data hello zapsaná náhodných zapisovačem hello.
* `sudoku`: Sudoku solver.
* `teragen`: Vygenerování dat pro hello terasort.
* `terasort`: Spusťte hello terasort.
* `teravalidate`: Kontrola výsledků terasort.
* `wordcount`: Mapreduce program, který udává hello slova v hello vstupní soubory.
* `wordmean`: Mapreduce program, který udává průměrnou délku hello hello slova v hello vstupní soubory.
* `wordmedian`: Mapreduce program, který udává hello Střední délka hello slova v hello vstupní soubory.
* `wordstandarddeviation`: Mapreduce program, který udává hello směrodatnou odchylku hello délka hello slova v hello vstupní soubory.

**Zdrojový kód**: obsahuje zdrojový kód pro tyto ukázky hello cluster HDInsight na `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

> [!NOTE]
> Hello `2.2.4.9-1` v hello cesta hello verzi hello softwaru Hortonworks Data Platform pro hello HDInsight cluster a mohou být různé pro váš cluster.

## <a name="run-hello-wordcount-example"></a>Spustit příklad wordcount hello

1. Připojte tooHDInsight pomocí protokolu SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Z hello `username@#######:~$` řádku, použijte následující příkaz toolist hello ukázky hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Tento příkaz generuje hello seznam ukázka z hello předchozí části tohoto dokumentu.

3. Hello použijte následující příkaz tooget nápovědy na konkrétní vzorku. V takovém případě hello **wordcount** ukázka:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Zobrazí hello následující zprávou:

        Usage: wordcount <in> [<in>...] <out>

    Tato zpráva znamená, můžete zadat několik vstupní cesta hello zdroj dokumenty. je posledním cesta Hello se uloží výstupní hello (počet slova v dokumentech zdroj hello).

4. Používejte následující toocount hello všechna slova v hello poznámkových bloků z Leonardo Da Vinci, které jsou k dispozici jako ukázková data k vašemu clusteru:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Zadejte pro tuto úlohu je pro čtení z `/example/data/gutenberg/davinci.txt`. Výstup pro tento příklad je uložené v `/example/data/davinciwordcount`. Obě cesty jsou umístěny na výchozí úložiště pro cluster hello, není hello místního systému souborů.

   > [!NOTE]
   > Jak jsme uvedli v hello nápovědy pro hello wordcount ukázku, můžete také zadat více vstupní soubory. Například `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` by počet slova v davinci.txt a ulysses.txt.

5. Po dokončení úlohy hello, použijte následující příkaz tooview hello výstup hello:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Tento příkaz připojí všechny hello výstupní soubory vytvořené úlohou hello. Zobrazuje konzoly toohello výstup hello. Hello výstup je podobné toohello následující text:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Každý řádek představuje slova a kolikrát došlo k chybě, a v hello vstupní data.

## <a name="hello-sudoku-example"></a>Příklad Sudoku Hello

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) je logiku stavebnice, tvoří devět 3 x 3 mřížky. Některé buňky v mřížce hello jsou čísla, zatímco jiné jsou prázdné a cílem hello je toosolve pro hello prázdné buňky. Hello předchozí odkaz obsahuje další informace o hello stavebnice, ale hello účelem Tato ukázka je toosolve pro hello prázdné buňky. Naše vstup, takže by měl být soubor, který je ve formátu hello:

* Devět řádků devět sloupců
* Každý sloupec může obsahovat buď v podobě čísla nebo `?` (což znamená prázdné buňky)
* Buňky jsou oddělené mezerou

Existuje určitá způsob tooconstruct hádanky vám Sudoku; nelze opakovat číslo sloupce nebo řádku. Je třeba na hello clusteru HDInsight, který je správně strukturován. Je umístěný na adrese `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` a obsahuje hello následující text:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

Použijte tento příklad problém prostřednictvím hello například Sudoku toorun hello následující příkaz:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

Hello výsledky se zobrazí podobné toohello následující text:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a>Příklad platformy (pí)

Ukázka pí Hello používá statistické (jako Monte Carlo) metoda tooestimate hello hodnotu čísla pí. Body jsou umístěny v náhodných v čtverce jednotky. hranaté Hello také obsahuje kruh. Hello pravděpodobnost, že body hello spadal do hello kruh jsou stejné toohello oblasti hello v kruhu, pí/4. z hodnoty hello 4R se dá odhadnout Hello hodnotu čísla pí. R je poměr hello hello počet bodů, které jsou uvnitř hello kruh toohello celkový počet bodů, které jsou v rámci hranaté hello. Hello větší hello ukázka bodů použít, je lepší odhad hello hello.

Tuto ukázku použijte následující příkaz toorun hello. Tento příkaz používá 16 mapy s 10 000 000 ukázky tooestimate hello hodnotu čísla pí:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

Hello hodnoty vrácené tento příkaz je podobný příliš**3.14159155000000000000**. Pro odkazy hello prvních 10 desetinných míst pí jsou 3.1415926535.

## <a name="10-gb-greysort-example"></a>Příklad Greysort 10 GB

GraySort je řazení srovnávacího testu. Metrika Hello je míra řazení hello (TB za minutu), které se dosáhne při řazení velkých objemů dat, obvykle 100 TB minimální.

Tato ukázka používá mírné 10 GB dat tak, aby mohl být program spuštěn poměrně rychle. Používá hello MapReduce aplikace vyvinuté tak, že Owen O'Malley a Arun Murthy. Tyto aplikace won hello roční pro obecné účely ("daytona") terabajt řazení srovnávací test v 2009, míru 0.578 TB za minutu (100 TB 173 minut). Další informace o tomto a dalších řazení srovnávacích testů najdete v tématu hello [Sortbenchmark](http://sortbenchmark.org/) lokality.

Tato ukázka používá tři sady MapReduce programů:

* **TeraGen**: A MapReduce program, který generuje řádky toosort dat

* **TeraSort**: ukázky hello vstupní data a používá MapReduce toosort hello data do celkové pořadí

    TeraSort je standardní řazení MapReduce, s výjimkou vlastní dělicí metody. dělicí metody Hello používá seřazený seznam klíčů N-1 vzorků, které definují hello klíče rozsah pro každý snižte. Konkrétně, všechny klíče takové které ukázkové [i-1] < = klíč < ukázka [i] jsou odesílány tooreduce i. Tato dělicí metody zaručuje, že hello výstupy snížit i jsou všechny menší než výstup hello snížit i + 1.

* **TeraValidate**: globálně A MapReduce program, který ověří tento výstup hello řazena

    Vytvoří jedna mapa každý soubor v adresáři výstup hello a každé mapování zajišťuje, že každý klíč je menší než nebo rovna toohello předchozí. Funkce mapy Hello generuje záznamy hello první a poslední klíče každého souboru. Hello snížit funkce zajišťuje, že hello první klíč souboru i je větší než hello poslední klíč i-1 souboru. Všechny problémy jsou hlášena jako výstup hello snížit fázi hello klíče, které jsou mimo pořadí.

Použití hello následující kroky toogenerate data, řazení a pak ověříte výstup hello:

1. Generovat 10 GB dat, což je výchozí úložiště clusteru HDInsight uložené toohello v `/example/data/10GB-sort-input`:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    Hello `-Dmapred.map.tasks` informuje Hadoop kolik toouse úlohy mapy pro tuto úlohu. Hello poslední dva parametry pokyn hello úlohy toocreate 10 GB dat a toostore v `/example/data/10GB-sort-input`.

2. Použijte následující příkaz toosort hello data hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    Hello `-Dmapred.reduce.tasks` informuje Hadoop, kolik snížit toouse úlohy pro úlohu hello. Hello poslední dva parametry jsou jenom hello vstupní a výstupní umístění pro data.

3. Použijte následující toovalidate hello data generována hello řazení hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Další kroky

V tomto článku jste se dozvěděli, jak toorun hello ukázky součástí hello clustery HDInsight se systémem Linux. Kurzech k používání Pig, Hive a MapReduce s HDInsight najdete v části hello následující témata:

* [Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]
* [Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]
* [Používání nástroje MapReduce s Hadoop v HDInsight][hdinsight-use-mapreduce]

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
