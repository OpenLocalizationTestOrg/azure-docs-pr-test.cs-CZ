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
# <a name="run-hello-mapreduce-examples-included-in-hdinsight"></a><span data-ttu-id="056db-105">Spuštění příkladů MapReduce hello zahrnuté v HDInsight</span><span class="sxs-lookup"><span data-stu-id="056db-105">Run hello MapReduce examples included in HDInsight</span></span>

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="056db-106">Zjistěte, jak toorun hello MapReduce příklady součástí Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="056db-106">Learn how toorun hello MapReduce examples included with Hadoop on HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="056db-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="056db-107">Prerequisites</span></span>

* <span data-ttu-id="056db-108">**Cluster služby HDInsight**: najdete v části [Začínáme pomocí Hadoop Hive v HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="056db-108">**An HDInsight cluster**: See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="056db-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="056db-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="056db-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="056db-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="056db-111">**Klientem SSH**: Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="056db-111">**An SSH client**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="hello-mapreduce-examples"></a><span data-ttu-id="056db-112">Příklady MapReduce Hello</span><span class="sxs-lookup"><span data-stu-id="056db-112">hello MapReduce examples</span></span>

<span data-ttu-id="056db-113">**Umístění**: hello ukázky jsou umístěny na hello cluster HDInsight na `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="056db-113">**Location**: hello samples are located on hello HDInsight cluster at `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span></span>

<span data-ttu-id="056db-114">**Obsah**: hello následující ukázky jsou obsaženy v archivu:</span><span class="sxs-lookup"><span data-stu-id="056db-114">**Contents**: hello following samples are contained in this archive:</span></span>

* <span data-ttu-id="056db-115">`aggregatewordcount`: Agregace na základě mapreduce program, který udává hello slova v hello vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="056db-115">`aggregatewordcount`: An Aggregate based mapreduce program that counts hello words in hello input files.</span></span>
* <span data-ttu-id="056db-116">`aggregatewordhist`: Agregace na základě mapreduce program, který vypočítá hello histogramu hello slova v hello vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="056db-116">`aggregatewordhist`: An Aggregate based mapreduce program that computes hello histogram of hello words in hello input files.</span></span>
* <span data-ttu-id="056db-117">`bbp`: Mapreduce program, který používá Baileyův. Borwein Plouffe toocompute přesný číslic čísla pí.</span><span class="sxs-lookup"><span data-stu-id="056db-117">`bbp`: A mapreduce program that uses Bailey-Borwein-Plouffe toocompute exact digits of Pi.</span></span>
* <span data-ttu-id="056db-118">`dbcount`: Příklad úlohu, která udává hello Stránkové zobrazení protokolů uložených v databázi.</span><span class="sxs-lookup"><span data-stu-id="056db-118">`dbcount`: An example job that counts hello pageview logs stored in a database.</span></span>
* <span data-ttu-id="056db-119">`distbbp`: Mapreduce program, který používá přesný bits BBP typ vzorce toocompute pí.</span><span class="sxs-lookup"><span data-stu-id="056db-119">`distbbp`: A mapreduce program that uses a BBP-type formula toocompute exact bits of Pi.</span></span>
* <span data-ttu-id="056db-120">`grep`: Mapreduce program, který udává hello odpovídá z regex ve vstupu hello.</span><span class="sxs-lookup"><span data-stu-id="056db-120">`grep`: A mapreduce program that counts hello matches of a regex in hello input.</span></span>
* <span data-ttu-id="056db-121">`join`: Úlohu, která provede spojení přes seřadit, rovnoměrně rozděleny do oddílů datových sad.</span><span class="sxs-lookup"><span data-stu-id="056db-121">`join`: A job that performs a join over sorted, equally partitioned datasets.</span></span>
* <span data-ttu-id="056db-122">`multifilewc`: Úlohu, která počty slov z několika souborů.</span><span class="sxs-lookup"><span data-stu-id="056db-122">`multifilewc`: A job that counts words from several files.</span></span>
* <span data-ttu-id="056db-123">`pentomino`: Dlaždici mapreduce, kterým program toofind řešení toopentomino problémy.</span><span class="sxs-lookup"><span data-stu-id="056db-123">`pentomino`: A mapreduce tile laying program toofind solutions toopentomino problems.</span></span>
* <span data-ttu-id="056db-124">`pi`: Mapreduce program, který odhadne pí pomocí jako Monte Carlo metoda.</span><span class="sxs-lookup"><span data-stu-id="056db-124">`pi`: A mapreduce program that estimates Pi using a quasi-Monte Carlo method.</span></span>
* <span data-ttu-id="056db-125">`randomtextwriter`: Mapreduce program, který zapíše 10 GB náhodné textový dat na jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="056db-125">`randomtextwriter`: A mapreduce program that writes 10 GB of random textual data per node.</span></span>
* <span data-ttu-id="056db-126">`randomwriter`: Mapreduce program, který zapíše 10 GB náhodná data na jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="056db-126">`randomwriter`: A mapreduce program that writes 10 GB of random data per node.</span></span>
* <span data-ttu-id="056db-127">`secondarysort`: Příklad definování sekundární řazení toohello snížit fáze.</span><span class="sxs-lookup"><span data-stu-id="056db-127">`secondarysort`: An example defining a secondary sort toohello reduce phase.</span></span>
* <span data-ttu-id="056db-128">`sort`: Mapreduce program, který seřadí data hello zapsaná náhodných zapisovačem hello.</span><span class="sxs-lookup"><span data-stu-id="056db-128">`sort`: A mapreduce program that sorts hello data written by hello random writer.</span></span>
* <span data-ttu-id="056db-129">`sudoku`: Sudoku solver.</span><span class="sxs-lookup"><span data-stu-id="056db-129">`sudoku`: A sudoku solver.</span></span>
* <span data-ttu-id="056db-130">`teragen`: Vygenerování dat pro hello terasort.</span><span class="sxs-lookup"><span data-stu-id="056db-130">`teragen`: Generate data for hello terasort.</span></span>
* <span data-ttu-id="056db-131">`terasort`: Spusťte hello terasort.</span><span class="sxs-lookup"><span data-stu-id="056db-131">`terasort`: Run hello terasort.</span></span>
* <span data-ttu-id="056db-132">`teravalidate`: Kontrola výsledků terasort.</span><span class="sxs-lookup"><span data-stu-id="056db-132">`teravalidate`: Checking results of terasort.</span></span>
* <span data-ttu-id="056db-133">`wordcount`: Mapreduce program, který udává hello slova v hello vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="056db-133">`wordcount`: A mapreduce program that counts hello words in hello input files.</span></span>
* <span data-ttu-id="056db-134">`wordmean`: Mapreduce program, který udává průměrnou délku hello hello slova v hello vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="056db-134">`wordmean`: A mapreduce program that counts hello average length of hello words in hello input files.</span></span>
* <span data-ttu-id="056db-135">`wordmedian`: Mapreduce program, který udává hello Střední délka hello slova v hello vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="056db-135">`wordmedian`: A mapreduce program that counts hello median length of hello words in hello input files.</span></span>
* <span data-ttu-id="056db-136">`wordstandarddeviation`: Mapreduce program, který udává hello směrodatnou odchylku hello délka hello slova v hello vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="056db-136">`wordstandarddeviation`: A mapreduce program that counts hello standard deviation of hello length of hello words in hello input files.</span></span>

<span data-ttu-id="056db-137">**Zdrojový kód**: obsahuje zdrojový kód pro tyto ukázky hello cluster HDInsight na `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span><span class="sxs-lookup"><span data-stu-id="056db-137">**Source code**: Source code for these samples is included on hello HDInsight cluster at `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span></span>

> [!NOTE]
> <span data-ttu-id="056db-138">Hello `2.2.4.9-1` v hello cesta hello verzi hello softwaru Hortonworks Data Platform pro hello HDInsight cluster a mohou být různé pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="056db-138">hello `2.2.4.9-1` in hello path is hello version of hello Hortonworks Data Platform for hello HDInsight cluster, and may be different for your cluster.</span></span>

## <a name="run-hello-wordcount-example"></a><span data-ttu-id="056db-139">Spustit příklad wordcount hello</span><span class="sxs-lookup"><span data-stu-id="056db-139">Run hello wordcount example</span></span>

1. <span data-ttu-id="056db-140">Připojte tooHDInsight pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="056db-140">Connect tooHDInsight using SSH.</span></span> <span data-ttu-id="056db-141">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="056db-141">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="056db-142">Z hello `username@#######:~$` řádku, použijte následující příkaz toolist hello ukázky hello:</span><span class="sxs-lookup"><span data-stu-id="056db-142">From hello `username@#######:~$` prompt, use hello following command toolist hello samples:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    <span data-ttu-id="056db-143">Tento příkaz generuje hello seznam ukázka z hello předchozí části tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="056db-143">This command generates hello list of sample from hello previous section of this document.</span></span>

3. <span data-ttu-id="056db-144">Hello použijte následující příkaz tooget nápovědy na konkrétní vzorku.</span><span class="sxs-lookup"><span data-stu-id="056db-144">Use hello following command tooget help on a specific sample.</span></span> <span data-ttu-id="056db-145">V takovém případě hello **wordcount** ukázka:</span><span class="sxs-lookup"><span data-stu-id="056db-145">In this case, hello **wordcount** sample:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    <span data-ttu-id="056db-146">Zobrazí hello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="056db-146">You receive hello following message:</span></span>

        Usage: wordcount <in> [<in>...] <out>

    <span data-ttu-id="056db-147">Tato zpráva znamená, můžete zadat několik vstupní cesta hello zdroj dokumenty.</span><span class="sxs-lookup"><span data-stu-id="056db-147">This message indicates that you can provide several input paths for hello source documents.</span></span> <span data-ttu-id="056db-148">je posledním cesta Hello se uloží výstupní hello (počet slova v dokumentech zdroj hello).</span><span class="sxs-lookup"><span data-stu-id="056db-148">hello final path is where hello output (count of words in hello source documents) is stored.</span></span>

4. <span data-ttu-id="056db-149">Používejte následující toocount hello všechna slova v hello poznámkových bloků z Leonardo Da Vinci, které jsou k dispozici jako ukázková data k vašemu clusteru:</span><span class="sxs-lookup"><span data-stu-id="056db-149">Use hello following toocount all words in hello Notebooks of Leonardo Da Vinci, which are provided as sample data with your cluster:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    <span data-ttu-id="056db-150">Zadejte pro tuto úlohu je pro čtení z `/example/data/gutenberg/davinci.txt`.</span><span class="sxs-lookup"><span data-stu-id="056db-150">Input for this job is read from `/example/data/gutenberg/davinci.txt`.</span></span> <span data-ttu-id="056db-151">Výstup pro tento příklad je uložené v `/example/data/davinciwordcount`.</span><span class="sxs-lookup"><span data-stu-id="056db-151">Output for this example is stored in `/example/data/davinciwordcount`.</span></span> <span data-ttu-id="056db-152">Obě cesty jsou umístěny na výchozí úložiště pro cluster hello, není hello místního systému souborů.</span><span class="sxs-lookup"><span data-stu-id="056db-152">Both paths are located on default storage for hello cluster, not hello local file system.</span></span>

   > [!NOTE]
   > <span data-ttu-id="056db-153">Jak jsme uvedli v hello nápovědy pro hello wordcount ukázku, můžete také zadat více vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="056db-153">As noted in hello help for hello wordcount sample, you could also specify multiple input files.</span></span> <span data-ttu-id="056db-154">Například `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` by počet slova v davinci.txt a ulysses.txt.</span><span class="sxs-lookup"><span data-stu-id="056db-154">For example, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` would count words in both davinci.txt and ulysses.txt.</span></span>

5. <span data-ttu-id="056db-155">Po dokončení úlohy hello, použijte následující příkaz tooview hello výstup hello:</span><span class="sxs-lookup"><span data-stu-id="056db-155">Once hello job completes, use hello following command tooview hello output:</span></span>

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    <span data-ttu-id="056db-156">Tento příkaz připojí všechny hello výstupní soubory vytvořené úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="056db-156">This command concatenates all hello output files produced by hello job.</span></span> <span data-ttu-id="056db-157">Zobrazuje konzoly toohello výstup hello.</span><span class="sxs-lookup"><span data-stu-id="056db-157">It displays hello output toohello console.</span></span> <span data-ttu-id="056db-158">Hello výstup je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="056db-158">hello output is similar toohello following text:</span></span>

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    <span data-ttu-id="056db-159">Každý řádek představuje slova a kolikrát došlo k chybě, a v hello vstupní data.</span><span class="sxs-lookup"><span data-stu-id="056db-159">Each line represents a word and how many times it occurred in hello input data.</span></span>

## <a name="hello-sudoku-example"></a><span data-ttu-id="056db-160">Příklad Sudoku Hello</span><span class="sxs-lookup"><span data-stu-id="056db-160">hello Sudoku example</span></span>

<span data-ttu-id="056db-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) je logiku stavebnice, tvoří devět 3 x 3 mřížky.</span><span class="sxs-lookup"><span data-stu-id="056db-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is a logic puzzle made up of nine 3x3 grids.</span></span> <span data-ttu-id="056db-162">Některé buňky v mřížce hello jsou čísla, zatímco jiné jsou prázdné a cílem hello je toosolve pro hello prázdné buňky.</span><span class="sxs-lookup"><span data-stu-id="056db-162">Some cells in hello grid have numbers, while others are blank, and hello goal is toosolve for hello blank cells.</span></span> <span data-ttu-id="056db-163">Hello předchozí odkaz obsahuje další informace o hello stavebnice, ale hello účelem Tato ukázka je toosolve pro hello prázdné buňky.</span><span class="sxs-lookup"><span data-stu-id="056db-163">hello previous link has more information on hello puzzle, but hello purpose of this sample is toosolve for hello blank cells.</span></span> <span data-ttu-id="056db-164">Naše vstup, takže by měl být soubor, který je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="056db-164">So our input should be a file that is in hello following format:</span></span>

* <span data-ttu-id="056db-165">Devět řádků devět sloupců</span><span class="sxs-lookup"><span data-stu-id="056db-165">Nine rows of nine columns</span></span>
* <span data-ttu-id="056db-166">Každý sloupec může obsahovat buď v podobě čísla nebo `?` (což znamená prázdné buňky)</span><span class="sxs-lookup"><span data-stu-id="056db-166">Each column can contain either a number or `?` (which indicates a blank cell)</span></span>
* <span data-ttu-id="056db-167">Buňky jsou oddělené mezerou</span><span class="sxs-lookup"><span data-stu-id="056db-167">Cells are separated by a space</span></span>

<span data-ttu-id="056db-168">Existuje určitá způsob tooconstruct hádanky vám Sudoku; nelze opakovat číslo sloupce nebo řádku.</span><span class="sxs-lookup"><span data-stu-id="056db-168">There is a certain way tooconstruct Sudoku puzzles; you can't repeat a number in a column or row.</span></span> <span data-ttu-id="056db-169">Je třeba na hello clusteru HDInsight, který je správně strukturován.</span><span class="sxs-lookup"><span data-stu-id="056db-169">There's an example on hello HDInsight cluster that is properly constructed.</span></span> <span data-ttu-id="056db-170">Je umístěný na adrese `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` a obsahuje hello následující text:</span><span class="sxs-lookup"><span data-stu-id="056db-170">It is located at `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` and contains hello following text:</span></span>

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

<span data-ttu-id="056db-171">Použijte tento příklad problém prostřednictvím hello například Sudoku toorun hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="056db-171">toorun this example problem through hello Sudoku example, use hello following command:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

<span data-ttu-id="056db-172">Hello výsledky se zobrazí podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="056db-172">hello results appear similar toohello following text:</span></span>

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a><span data-ttu-id="056db-173">Příklad platformy (pí)</span><span class="sxs-lookup"><span data-stu-id="056db-173">Pi (π) example</span></span>

<span data-ttu-id="056db-174">Ukázka pí Hello používá statistické (jako Monte Carlo) metoda tooestimate hello hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="056db-174">hello pi sample uses a statistical (quasi-Monte Carlo) method tooestimate hello value of pi.</span></span> <span data-ttu-id="056db-175">Body jsou umístěny v náhodných v čtverce jednotky.</span><span class="sxs-lookup"><span data-stu-id="056db-175">Points are placed at random in a unit square.</span></span> <span data-ttu-id="056db-176">hranaté Hello také obsahuje kruh.</span><span class="sxs-lookup"><span data-stu-id="056db-176">hello square also contains a circle.</span></span> <span data-ttu-id="056db-177">Hello pravděpodobnost, že body hello spadal do hello kruh jsou stejné toohello oblasti hello v kruhu, pí/4.</span><span class="sxs-lookup"><span data-stu-id="056db-177">hello probability that hello points fall within hello circle are equal toohello area of hello circle, pi/4.</span></span> <span data-ttu-id="056db-178">z hodnoty hello 4R se dá odhadnout Hello hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="056db-178">hello value of pi can be estimated from hello value of 4R.</span></span> <span data-ttu-id="056db-179">R je poměr hello hello počet bodů, které jsou uvnitř hello kruh toohello celkový počet bodů, které jsou v rámci hranaté hello.</span><span class="sxs-lookup"><span data-stu-id="056db-179">R is hello ratio of hello number of points that are inside hello circle toohello total number of points that are within hello square.</span></span> <span data-ttu-id="056db-180">Hello větší hello ukázka bodů použít, je lepší odhad hello hello.</span><span class="sxs-lookup"><span data-stu-id="056db-180">hello larger hello sample of points used, hello better hello estimate is.</span></span>

<span data-ttu-id="056db-181">Tuto ukázku použijte následující příkaz toorun hello.</span><span class="sxs-lookup"><span data-stu-id="056db-181">Use hello following command toorun this sample.</span></span> <span data-ttu-id="056db-182">Tento příkaz používá 16 mapy s 10 000 000 ukázky tooestimate hello hodnotu čísla pí:</span><span class="sxs-lookup"><span data-stu-id="056db-182">This command uses 16 maps with 10,000,000 samples each tooestimate hello value of pi:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

<span data-ttu-id="056db-183">Hello hodnoty vrácené tento příkaz je podobný příliš**3.14159155000000000000**.</span><span class="sxs-lookup"><span data-stu-id="056db-183">hello value returned by this command is similar too**3.14159155000000000000**.</span></span> <span data-ttu-id="056db-184">Pro odkazy hello prvních 10 desetinných míst pí jsou 3.1415926535.</span><span class="sxs-lookup"><span data-stu-id="056db-184">For references, hello first 10 decimal places of pi are 3.1415926535.</span></span>

## <a name="10-gb-greysort-example"></a><span data-ttu-id="056db-185">Příklad Greysort 10 GB</span><span class="sxs-lookup"><span data-stu-id="056db-185">10 GB Greysort example</span></span>

<span data-ttu-id="056db-186">GraySort je řazení srovnávacího testu.</span><span class="sxs-lookup"><span data-stu-id="056db-186">GraySort is a benchmark sort.</span></span> <span data-ttu-id="056db-187">Metrika Hello je míra řazení hello (TB za minutu), které se dosáhne při řazení velkých objemů dat, obvykle 100 TB minimální.</span><span class="sxs-lookup"><span data-stu-id="056db-187">hello metric is hello sort rate (TB/minute) that is achieved while sorting large amounts of data, usually a 100 TB minimum.</span></span>

<span data-ttu-id="056db-188">Tato ukázka používá mírné 10 GB dat tak, aby mohl být program spuštěn poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="056db-188">This sample uses a modest 10 GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="056db-189">Používá hello MapReduce aplikace vyvinuté tak, že Owen O'Malley a Arun Murthy.</span><span class="sxs-lookup"><span data-stu-id="056db-189">It uses hello MapReduce applications developed by Owen O'Malley and Arun Murthy.</span></span> <span data-ttu-id="056db-190">Tyto aplikace won hello roční pro obecné účely ("daytona") terabajt řazení srovnávací test v 2009, míru 0.578 TB za minutu (100 TB 173 minut).</span><span class="sxs-lookup"><span data-stu-id="056db-190">These applications won hello annual general-purpose ("daytona") terabyte sort benchmark in 2009, with a rate of 0.578 TB/min (100 TB in 173 minutes).</span></span> <span data-ttu-id="056db-191">Další informace o tomto a dalších řazení srovnávacích testů najdete v tématu hello [Sortbenchmark](http://sortbenchmark.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="056db-191">For more information on this and other sorting benchmarks, see hello [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="056db-192">Tato ukázka používá tři sady MapReduce programů:</span><span class="sxs-lookup"><span data-stu-id="056db-192">This sample uses three sets of MapReduce programs:</span></span>

* <span data-ttu-id="056db-193">**TeraGen**: A MapReduce program, který generuje řádky toosort dat</span><span class="sxs-lookup"><span data-stu-id="056db-193">**TeraGen**: A MapReduce program that generates rows of data toosort</span></span>

* <span data-ttu-id="056db-194">**TeraSort**: ukázky hello vstupní data a používá MapReduce toosort hello data do celkové pořadí</span><span class="sxs-lookup"><span data-stu-id="056db-194">**TeraSort**: Samples hello input data and uses MapReduce toosort hello data into a total order</span></span>

    <span data-ttu-id="056db-195">TeraSort je standardní řazení MapReduce, s výjimkou vlastní dělicí metody.</span><span class="sxs-lookup"><span data-stu-id="056db-195">TeraSort is a standard MapReduce sort, except for a custom partitioner.</span></span> <span data-ttu-id="056db-196">dělicí metody Hello používá seřazený seznam klíčů N-1 vzorků, které definují hello klíče rozsah pro každý snižte.</span><span class="sxs-lookup"><span data-stu-id="056db-196">hello partitioner uses a sorted list of N-1 sampled keys that define hello key range for each reduce.</span></span> <span data-ttu-id="056db-197">Konkrétně, všechny klíče takové které ukázkové [i-1] < = klíč < ukázka [i] jsou odesílány tooreduce i.</span><span class="sxs-lookup"><span data-stu-id="056db-197">In particular, all keys such that sample[i-1] <= key < sample[i] are sent tooreduce i.</span></span> <span data-ttu-id="056db-198">Tato dělicí metody zaručuje, že hello výstupy snížit i jsou všechny menší než výstup hello snížit i + 1.</span><span class="sxs-lookup"><span data-stu-id="056db-198">This partitioner guarantees that hello outputs of reduce i are all less than hello output of reduce i+1.</span></span>

* <span data-ttu-id="056db-199">**TeraValidate**: globálně A MapReduce program, který ověří tento výstup hello řazena</span><span class="sxs-lookup"><span data-stu-id="056db-199">**TeraValidate**: A MapReduce program that validates that hello output is globally sorted</span></span>

    <span data-ttu-id="056db-200">Vytvoří jedna mapa každý soubor v adresáři výstup hello a každé mapování zajišťuje, že každý klíč je menší než nebo rovna toohello předchozí.</span><span class="sxs-lookup"><span data-stu-id="056db-200">It creates one map per file in hello output directory, and each map ensures that each key is less than or equal toohello previous one.</span></span> <span data-ttu-id="056db-201">Funkce mapy Hello generuje záznamy hello první a poslední klíče každého souboru.</span><span class="sxs-lookup"><span data-stu-id="056db-201">hello map function generates records of hello first and last keys of each file.</span></span> <span data-ttu-id="056db-202">Hello snížit funkce zajišťuje, že hello první klíč souboru i je větší než hello poslední klíč i-1 souboru.</span><span class="sxs-lookup"><span data-stu-id="056db-202">hello reduce function ensures that hello first key of file i is greater than hello last key of file i-1.</span></span> <span data-ttu-id="056db-203">Všechny problémy jsou hlášena jako výstup hello snížit fázi hello klíče, které jsou mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="056db-203">Any problems are reported as an output of hello reduce phase, with hello keys that are out of order.</span></span>

<span data-ttu-id="056db-204">Použití hello následující kroky toogenerate data, řazení a pak ověříte výstup hello:</span><span class="sxs-lookup"><span data-stu-id="056db-204">Use hello following steps toogenerate data, sort, and then validate hello output:</span></span>

1. <span data-ttu-id="056db-205">Generovat 10 GB dat, což je výchozí úložiště clusteru HDInsight uložené toohello v `/example/data/10GB-sort-input`:</span><span class="sxs-lookup"><span data-stu-id="056db-205">Generate 10 GB of data, which is stored toohello HDInsight cluster's default storage at `/example/data/10GB-sort-input`:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    <span data-ttu-id="056db-206">Hello `-Dmapred.map.tasks` informuje Hadoop kolik toouse úlohy mapy pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="056db-206">hello `-Dmapred.map.tasks` tells Hadoop how many map tasks toouse for this job.</span></span> <span data-ttu-id="056db-207">Hello poslední dva parametry pokyn hello úlohy toocreate 10 GB dat a toostore v `/example/data/10GB-sort-input`.</span><span class="sxs-lookup"><span data-stu-id="056db-207">hello final two parameters instruct hello job toocreate 10 GB of data and toostore it at `/example/data/10GB-sort-input`.</span></span>

2. <span data-ttu-id="056db-208">Použijte následující příkaz toosort hello data hello:</span><span class="sxs-lookup"><span data-stu-id="056db-208">Use hello following command toosort hello data:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    <span data-ttu-id="056db-209">Hello `-Dmapred.reduce.tasks` informuje Hadoop, kolik snížit toouse úlohy pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="056db-209">hello `-Dmapred.reduce.tasks` tells Hadoop how many reduce tasks toouse for hello job.</span></span> <span data-ttu-id="056db-210">Hello poslední dva parametry jsou jenom hello vstupní a výstupní umístění pro data.</span><span class="sxs-lookup"><span data-stu-id="056db-210">hello final two parameters are just hello input and output locations for data.</span></span>

3. <span data-ttu-id="056db-211">Použijte následující toovalidate hello data generována hello řazení hello:</span><span class="sxs-lookup"><span data-stu-id="056db-211">Use hello following toovalidate hello data generated by hello sort:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a><span data-ttu-id="056db-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="056db-212">Next steps</span></span>

<span data-ttu-id="056db-213">V tomto článku jste se dozvěděli, jak toorun hello ukázky součástí hello clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="056db-213">From this article, you learned how toorun hello samples included with hello Linux-based HDInsight clusters.</span></span> <span data-ttu-id="056db-214">Kurzech k používání Pig, Hive a MapReduce s HDInsight najdete v části hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="056db-214">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see hello following topics:</span></span>

* <span data-ttu-id="056db-215">[Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="056db-215">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="056db-216">[Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="056db-216">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="056db-217">[Používání nástroje MapReduce s Hadoop v HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="056db-217">[Use MapReduce with Hadoop on HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
