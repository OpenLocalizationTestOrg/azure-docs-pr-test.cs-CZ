---
title: "Spuštění příkladů MapReduce s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Začněte používat MapReduce ukázky v jar souborů zahrnutých v HDInsight. Připojte se ke clusteru pomocí SSH a pak použít příkaz Hadoop ke spuštění ukázkové úlohy."
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
ms.openlocfilehash: 447c07f869ff9a2a2a00089248be98e6729d6dc4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a><span data-ttu-id="ddce6-105">Spuštění příkladů MapReduce zahrnuté v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ddce6-105">Run the MapReduce examples included in HDInsight</span></span>

[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="ddce6-106">Informace o spouštění příklady MapReduce součástí Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ddce6-106">Learn how to run the MapReduce examples included with Hadoop on HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ddce6-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ddce6-107">Prerequisites</span></span>

* <span data-ttu-id="ddce6-108">**Cluster služby HDInsight**: najdete v části [Začínáme pomocí Hadoop Hive v HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="ddce6-108">**An HDInsight cluster**: See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ddce6-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ddce6-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ddce6-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ddce6-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="ddce6-111">**Klientem SSH**: Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ddce6-111">**An SSH client**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="the-mapreduce-examples"></a><span data-ttu-id="ddce6-112">Příklady MapReduce</span><span class="sxs-lookup"><span data-stu-id="ddce6-112">The MapReduce examples</span></span>

<span data-ttu-id="ddce6-113">**Umístění**: jsou ukázky umístěny v clusteru HDInsight na `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="ddce6-113">**Location**: The samples are located on the HDInsight cluster at `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.</span></span>

<span data-ttu-id="ddce6-114">**Obsah**: následující ukázky jsou obsaženy v archivu:</span><span class="sxs-lookup"><span data-stu-id="ddce6-114">**Contents**: The following samples are contained in this archive:</span></span>

* <span data-ttu-id="ddce6-115">`aggregatewordcount`: Agregace na základě mapreduce program, který udává slova ve vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ddce6-115">`aggregatewordcount`: An Aggregate based mapreduce program that counts the words in the input files.</span></span>
* <span data-ttu-id="ddce6-116">`aggregatewordhist`: Agregace na základě mapreduce program, který vypočítá histogramu slova ve vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ddce6-116">`aggregatewordhist`: An Aggregate based mapreduce program that computes the histogram of the words in the input files.</span></span>
* <span data-ttu-id="ddce6-117">`bbp`: Mapreduce program, který používá Baileyův. Borwein Plouffe k výpočtu přesný číslic čísla pí.</span><span class="sxs-lookup"><span data-stu-id="ddce6-117">`bbp`: A mapreduce program that uses Bailey-Borwein-Plouffe to compute exact digits of Pi.</span></span>
* <span data-ttu-id="ddce6-118">`dbcount`: Příklad úlohu, která udává Stránkové zobrazení protokolů uložených v databázi.</span><span class="sxs-lookup"><span data-stu-id="ddce6-118">`dbcount`: An example job that counts the pageview logs stored in a database.</span></span>
* <span data-ttu-id="ddce6-119">`distbbp`: Mapreduce program, který používá typ BBP vzorec k výpočtu přesný bits pí.</span><span class="sxs-lookup"><span data-stu-id="ddce6-119">`distbbp`: A mapreduce program that uses a BBP-type formula to compute exact bits of Pi.</span></span>
* <span data-ttu-id="ddce6-120">`grep`: Mapreduce program, který počty shod regex ve vstupu.</span><span class="sxs-lookup"><span data-stu-id="ddce6-120">`grep`: A mapreduce program that counts the matches of a regex in the input.</span></span>
* <span data-ttu-id="ddce6-121">`join`: Úlohu, která provede spojení přes seřadit, rovnoměrně rozděleny do oddílů datových sad.</span><span class="sxs-lookup"><span data-stu-id="ddce6-121">`join`: A job that performs a join over sorted, equally partitioned datasets.</span></span>
* <span data-ttu-id="ddce6-122">`multifilewc`: Úlohu, která počty slov z několika souborů.</span><span class="sxs-lookup"><span data-stu-id="ddce6-122">`multifilewc`: A job that counts words from several files.</span></span>
* <span data-ttu-id="ddce6-123">`pentomino`: Mapreduce dlaždice, kterým program k řešení problémů pentomino najít.</span><span class="sxs-lookup"><span data-stu-id="ddce6-123">`pentomino`: A mapreduce tile laying program to find solutions to pentomino problems.</span></span>
* <span data-ttu-id="ddce6-124">`pi`: Mapreduce program, který odhadne pí pomocí jako Monte Carlo metoda.</span><span class="sxs-lookup"><span data-stu-id="ddce6-124">`pi`: A mapreduce program that estimates Pi using a quasi-Monte Carlo method.</span></span>
* <span data-ttu-id="ddce6-125">`randomtextwriter`: Mapreduce program, který zapíše 10 GB náhodné textový dat na jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="ddce6-125">`randomtextwriter`: A mapreduce program that writes 10 GB of random textual data per node.</span></span>
* <span data-ttu-id="ddce6-126">`randomwriter`: Mapreduce program, který zapíše 10 GB náhodná data na jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="ddce6-126">`randomwriter`: A mapreduce program that writes 10 GB of random data per node.</span></span>
* <span data-ttu-id="ddce6-127">`secondarysort`: Příklad definování sekundární řazení do fáze snižte.</span><span class="sxs-lookup"><span data-stu-id="ddce6-127">`secondarysort`: An example defining a secondary sort to the reduce phase.</span></span>
* <span data-ttu-id="ddce6-128">`sort`: Mapreduce program, který seřadí data zapsaná náhodný zápis.</span><span class="sxs-lookup"><span data-stu-id="ddce6-128">`sort`: A mapreduce program that sorts the data written by the random writer.</span></span>
* <span data-ttu-id="ddce6-129">`sudoku`: Sudoku solver.</span><span class="sxs-lookup"><span data-stu-id="ddce6-129">`sudoku`: A sudoku solver.</span></span>
* <span data-ttu-id="ddce6-130">`teragen`: Vygenerování dat pro terasort.</span><span class="sxs-lookup"><span data-stu-id="ddce6-130">`teragen`: Generate data for the terasort.</span></span>
* <span data-ttu-id="ddce6-131">`terasort`: Spusťte terasort.</span><span class="sxs-lookup"><span data-stu-id="ddce6-131">`terasort`: Run the terasort.</span></span>
* <span data-ttu-id="ddce6-132">`teravalidate`: Kontrola výsledků terasort.</span><span class="sxs-lookup"><span data-stu-id="ddce6-132">`teravalidate`: Checking results of terasort.</span></span>
* <span data-ttu-id="ddce6-133">`wordcount`: Mapreduce program, který udává slova ve vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ddce6-133">`wordcount`: A mapreduce program that counts the words in the input files.</span></span>
* <span data-ttu-id="ddce6-134">`wordmean`: Mapreduce program, který udává průměrnou délku slova ve vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ddce6-134">`wordmean`: A mapreduce program that counts the average length of the words in the input files.</span></span>
* <span data-ttu-id="ddce6-135">`wordmedian`: Mapreduce program, který udává Střední délka slova ve vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ddce6-135">`wordmedian`: A mapreduce program that counts the median length of the words in the input files.</span></span>
* <span data-ttu-id="ddce6-136">`wordstandarddeviation`: Mapreduce program, který udává směrodatnou odchylku Délka slova ve vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ddce6-136">`wordstandarddeviation`: A mapreduce program that counts the standard deviation of the length of the words in the input files.</span></span>

<span data-ttu-id="ddce6-137">**Zdrojový kód**: zdrojový kód pro tyto ukázky je zahrnuta v clusteru HDInsight na `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span><span class="sxs-lookup"><span data-stu-id="ddce6-137">**Source code**: Source code for these samples is included on the HDInsight cluster at `/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.</span></span>

> [!NOTE]
> <span data-ttu-id="ddce6-138">`2.2.4.9-1` v cestě je verze softwaru Hortonworks Data Platform pro HDInsight cluster a mohou být různé pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="ddce6-138">The `2.2.4.9-1` in the path is the version of the Hortonworks Data Platform for the HDInsight cluster, and may be different for your cluster.</span></span>

## <a name="run-the-wordcount-example"></a><span data-ttu-id="ddce6-139">Spustit příklad wordcount</span><span class="sxs-lookup"><span data-stu-id="ddce6-139">Run the wordcount example</span></span>

1. <span data-ttu-id="ddce6-140">Připojení k HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="ddce6-140">Connect to HDInsight using SSH.</span></span> <span data-ttu-id="ddce6-141">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ddce6-141">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="ddce6-142">Z `username@#######:~$` řádku, použijte následující příkaz k zobrazení seznamu ukázky:</span><span class="sxs-lookup"><span data-stu-id="ddce6-142">From the `username@#######:~$` prompt, use the following command to list the samples:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    <span data-ttu-id="ddce6-143">Tento příkaz vytvoří seznam ukázka z předchozí části tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ddce6-143">This command generates the list of sample from the previous section of this document.</span></span>

3. <span data-ttu-id="ddce6-144">Použijte následující příkaz k získání nápovědy na konkrétní vzorku.</span><span class="sxs-lookup"><span data-stu-id="ddce6-144">Use the following command to get help on a specific sample.</span></span> <span data-ttu-id="ddce6-145">V takovém případě **wordcount** ukázka:</span><span class="sxs-lookup"><span data-stu-id="ddce6-145">In this case, the **wordcount** sample:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    <span data-ttu-id="ddce6-146">Zobrazí se následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="ddce6-146">You receive the following message:</span></span>

        Usage: wordcount <in> [<in>...] <out>

    <span data-ttu-id="ddce6-147">Tato zpráva znamená, můžete zadat několik cest vstupní zdroj dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ddce6-147">This message indicates that you can provide several input paths for the source documents.</span></span> <span data-ttu-id="ddce6-148">Konečné cesty je, se uloží výstupní (počet slova v dokumentech zdroje).</span><span class="sxs-lookup"><span data-stu-id="ddce6-148">The final path is where the output (count of words in the source documents) is stored.</span></span>

4. <span data-ttu-id="ddce6-149">Počítat všechny slova v poznámkových bloků z Leonardo Da Vinci, které jsou k dispozici jako ukázková data k vašemu clusteru použijte následující:</span><span class="sxs-lookup"><span data-stu-id="ddce6-149">Use the following to count all words in the Notebooks of Leonardo Da Vinci, which are provided as sample data with your cluster:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    <span data-ttu-id="ddce6-150">Zadejte pro tuto úlohu je pro čtení z `/example/data/gutenberg/davinci.txt`.</span><span class="sxs-lookup"><span data-stu-id="ddce6-150">Input for this job is read from `/example/data/gutenberg/davinci.txt`.</span></span> <span data-ttu-id="ddce6-151">Výstup pro tento příklad je uložené v `/example/data/davinciwordcount`.</span><span class="sxs-lookup"><span data-stu-id="ddce6-151">Output for this example is stored in `/example/data/davinciwordcount`.</span></span> <span data-ttu-id="ddce6-152">Obě cesty jsou umístěny na výchozí úložiště pro cluster, není místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="ddce6-152">Both paths are located on default storage for the cluster, not the local file system.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ddce6-153">Jak jsme uvedli v nápovědě pro ukázku wordcount, můžete také zadat více vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ddce6-153">As noted in the help for the wordcount sample, you could also specify multiple input files.</span></span> <span data-ttu-id="ddce6-154">Například `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` by počet slova v davinci.txt a ulysses.txt.</span><span class="sxs-lookup"><span data-stu-id="ddce6-154">For example, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` would count words in both davinci.txt and ulysses.txt.</span></span>

5. <span data-ttu-id="ddce6-155">Po dokončení úlohy pro zobrazení výstupu použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ddce6-155">Once the job completes, use the following command to view the output:</span></span>

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    <span data-ttu-id="ddce6-156">Tento příkaz připojí všechny výstupní soubory vytvořené úlohou.</span><span class="sxs-lookup"><span data-stu-id="ddce6-156">This command concatenates all the output files produced by the job.</span></span> <span data-ttu-id="ddce6-157">Zobrazuje výstup do konzoly.</span><span class="sxs-lookup"><span data-stu-id="ddce6-157">It displays the output to the console.</span></span> <span data-ttu-id="ddce6-158">Výstup se bude podobat následujícímu:</span><span class="sxs-lookup"><span data-stu-id="ddce6-158">The output is similar to the following text:</span></span>

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    <span data-ttu-id="ddce6-159">Každý řádek představuje slova a kolikrát došlo v vstupní data.</span><span class="sxs-lookup"><span data-stu-id="ddce6-159">Each line represents a word and how many times it occurred in the input data.</span></span>

## <a name="the-sudoku-example"></a><span data-ttu-id="ddce6-160">Příklad Sudoku</span><span class="sxs-lookup"><span data-stu-id="ddce6-160">The Sudoku example</span></span>

<span data-ttu-id="ddce6-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) je logiku stavebnice, tvoří devět 3 x 3 mřížky.</span><span class="sxs-lookup"><span data-stu-id="ddce6-161">[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is a logic puzzle made up of nine 3x3 grids.</span></span> <span data-ttu-id="ddce6-162">Některé buněk v mřížce mít čísel, zatímco jiné jsou prázdné a cílem je řešení pro prázdné buňky.</span><span class="sxs-lookup"><span data-stu-id="ddce6-162">Some cells in the grid have numbers, while others are blank, and the goal is to solve for the blank cells.</span></span> <span data-ttu-id="ddce6-163">Předchozí odkaz obsahuje další informace o stavebnice, ale tato ukázka má řešení pro prázdné buňky.</span><span class="sxs-lookup"><span data-stu-id="ddce6-163">The previous link has more information on the puzzle, but the purpose of this sample is to solve for the blank cells.</span></span> <span data-ttu-id="ddce6-164">Naše vstup, takže by měl být soubor, který je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="ddce6-164">So our input should be a file that is in the following format:</span></span>

* <span data-ttu-id="ddce6-165">Devět řádků devět sloupců</span><span class="sxs-lookup"><span data-stu-id="ddce6-165">Nine rows of nine columns</span></span>
* <span data-ttu-id="ddce6-166">Každý sloupec může obsahovat buď v podobě čísla nebo `?` (což znamená prázdné buňky)</span><span class="sxs-lookup"><span data-stu-id="ddce6-166">Each column can contain either a number or `?` (which indicates a blank cell)</span></span>
* <span data-ttu-id="ddce6-167">Buňky jsou oddělené mezerou</span><span class="sxs-lookup"><span data-stu-id="ddce6-167">Cells are separated by a space</span></span>

<span data-ttu-id="ddce6-168">Tento problém je možné vytvořit hádanky vám Sudoku; nelze opakovat číslo sloupce nebo řádku.</span><span class="sxs-lookup"><span data-stu-id="ddce6-168">There is a certain way to construct Sudoku puzzles; you can't repeat a number in a column or row.</span></span> <span data-ttu-id="ddce6-169">Je třeba na clusteru HDInsight, který je správně strukturován.</span><span class="sxs-lookup"><span data-stu-id="ddce6-169">There's an example on the HDInsight cluster that is properly constructed.</span></span> <span data-ttu-id="ddce6-170">Je umístěný na adrese `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` a obsahuje následující text:</span><span class="sxs-lookup"><span data-stu-id="ddce6-170">It is located at `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` and contains the following text:</span></span>

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

<span data-ttu-id="ddce6-171">Pokud chcete spustit tento příklad problém prostřednictvím příklad Sudoku, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ddce6-171">To run this example problem through the Sudoku example, use the following command:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

<span data-ttu-id="ddce6-172">Výsledky se zobrazí podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="ddce6-172">The results appear similar to the following text:</span></span>

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a><span data-ttu-id="ddce6-173">Příklad platformy (pí)</span><span class="sxs-lookup"><span data-stu-id="ddce6-173">Pi (π) example</span></span>

<span data-ttu-id="ddce6-174">Ukázka pí používá statistické (jako Monte Carlo) metoda odhadnout hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="ddce6-174">The pi sample uses a statistical (quasi-Monte Carlo) method to estimate the value of pi.</span></span> <span data-ttu-id="ddce6-175">Body jsou umístěny v náhodných v čtverce jednotky.</span><span class="sxs-lookup"><span data-stu-id="ddce6-175">Points are placed at random in a unit square.</span></span> <span data-ttu-id="ddce6-176">Druhou mocninu také obsahuje kruh.</span><span class="sxs-lookup"><span data-stu-id="ddce6-176">The square also contains a circle.</span></span> <span data-ttu-id="ddce6-177">Pravděpodobnost, že body spadal do kruhu jsou stejné pro oblast na kruh pí/4.</span><span class="sxs-lookup"><span data-stu-id="ddce6-177">The probability that the points fall within the circle are equal to the area of the circle, pi/4.</span></span> <span data-ttu-id="ddce6-178">Z hodnoty 4R se dá odhadnout hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="ddce6-178">The value of pi can be estimated from the value of 4R.</span></span> <span data-ttu-id="ddce6-179">R je poměr počtu bodů, které jsou uvnitř kruhu celkového počtu bodů, které jsou v rámci druhou mocninu.</span><span class="sxs-lookup"><span data-stu-id="ddce6-179">R is the ratio of the number of points that are inside the circle to the total number of points that are within the square.</span></span> <span data-ttu-id="ddce6-180">Větší ukázkové bodů použít, tím lepší odhad je.</span><span class="sxs-lookup"><span data-stu-id="ddce6-180">The larger the sample of points used, the better the estimate is.</span></span>

<span data-ttu-id="ddce6-181">Pokud chcete tuto ukázku spustit, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="ddce6-181">Use the following command to run this sample.</span></span> <span data-ttu-id="ddce6-182">Tento příkaz používá 16 mapy s 10 000 000 ukázky odhadnout hodnotu čísla pí:</span><span class="sxs-lookup"><span data-stu-id="ddce6-182">This command uses 16 maps with 10,000,000 samples each to estimate the value of pi:</span></span>

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

<span data-ttu-id="ddce6-183">Hodnoty vrácené tento příkaz je podobná **3.14159155000000000000**.</span><span class="sxs-lookup"><span data-stu-id="ddce6-183">The value returned by this command is similar to **3.14159155000000000000**.</span></span> <span data-ttu-id="ddce6-184">Pro odkazy jsou prvních 10 desetinných míst pí 3.1415926535.</span><span class="sxs-lookup"><span data-stu-id="ddce6-184">For references, the first 10 decimal places of pi are 3.1415926535.</span></span>

## <a name="10-gb-greysort-example"></a><span data-ttu-id="ddce6-185">Příklad Greysort 10 GB</span><span class="sxs-lookup"><span data-stu-id="ddce6-185">10 GB Greysort example</span></span>

<span data-ttu-id="ddce6-186">GraySort je řazení srovnávacího testu.</span><span class="sxs-lookup"><span data-stu-id="ddce6-186">GraySort is a benchmark sort.</span></span> <span data-ttu-id="ddce6-187">Metrika je míra řazení (TB za minutu), které se dosáhne při řazení velkých objemů dat, obvykle 100 TB minimální.</span><span class="sxs-lookup"><span data-stu-id="ddce6-187">The metric is the sort rate (TB/minute) that is achieved while sorting large amounts of data, usually a 100 TB minimum.</span></span>

<span data-ttu-id="ddce6-188">Tato ukázka používá mírné 10 GB dat tak, aby mohl být program spuštěn poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="ddce6-188">This sample uses a modest 10 GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="ddce6-189">Používá MapReduce aplikace vyvinuté tak, že Owen O'Malley a Arun Murthy.</span><span class="sxs-lookup"><span data-stu-id="ddce6-189">It uses the MapReduce applications developed by Owen O'Malley and Arun Murthy.</span></span> <span data-ttu-id="ddce6-190">Tyto aplikace won roční srovnávacího testu řazení terabajt pro obecné účely ("daytona") v 2009, míru 0.578 TB za minutu (100 TB 173 minut).</span><span class="sxs-lookup"><span data-stu-id="ddce6-190">These applications won the annual general-purpose ("daytona") terabyte sort benchmark in 2009, with a rate of 0.578 TB/min (100 TB in 173 minutes).</span></span> <span data-ttu-id="ddce6-191">Další informace o tomto a dalších řazení srovnávacích testů najdete v tématu [Sortbenchmark](http://sortbenchmark.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="ddce6-191">For more information on this and other sorting benchmarks, see the [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="ddce6-192">Tato ukázka používá tři sady MapReduce programů:</span><span class="sxs-lookup"><span data-stu-id="ddce6-192">This sample uses three sets of MapReduce programs:</span></span>

* <span data-ttu-id="ddce6-193">**TeraGen**: A MapReduce program, který generuje řádky dat. Chcete-li seřadit</span><span class="sxs-lookup"><span data-stu-id="ddce6-193">**TeraGen**: A MapReduce program that generates rows of data to sort</span></span>

* <span data-ttu-id="ddce6-194">**TeraSort**: ukázky vstupní data a používá MapReduce k řazení dat. do celkové pořadí</span><span class="sxs-lookup"><span data-stu-id="ddce6-194">**TeraSort**: Samples the input data and uses MapReduce to sort the data into a total order</span></span>

    <span data-ttu-id="ddce6-195">TeraSort je standardní řazení MapReduce, s výjimkou vlastní dělicí metody.</span><span class="sxs-lookup"><span data-stu-id="ddce6-195">TeraSort is a standard MapReduce sort, except for a custom partitioner.</span></span> <span data-ttu-id="ddce6-196">Dělicí metody používá seřazený seznam klíčů N-1 vzorků, které definují rozsah klíče pro každý snižte.</span><span class="sxs-lookup"><span data-stu-id="ddce6-196">The partitioner uses a sorted list of N-1 sampled keys that define the key range for each reduce.</span></span> <span data-ttu-id="ddce6-197">Konkrétně, všechny klíče takové které ukázkové [i-1] < = klíč < ukázka [i] odešlou ke snížení i.</span><span class="sxs-lookup"><span data-stu-id="ddce6-197">In particular, all keys such that sample[i-1] <= key < sample[i] are sent to reduce i.</span></span> <span data-ttu-id="ddce6-198">Tato dělicí metody zaručuje, že výstupy snížit i jsou všechny menší než výstup snížit i + 1.</span><span class="sxs-lookup"><span data-stu-id="ddce6-198">This partitioner guarantees that the outputs of reduce i are all less than the output of reduce i+1.</span></span>

* <span data-ttu-id="ddce6-199">**TeraValidate**: A MapReduce program, který ověří, že výstup globálně seřazen</span><span class="sxs-lookup"><span data-stu-id="ddce6-199">**TeraValidate**: A MapReduce program that validates that the output is globally sorted</span></span>

    <span data-ttu-id="ddce6-200">Vytvoří jedna mapa každý soubor v adresáři výstup a každé mapování zajišťuje, že každý klíč je menší než nebo rovna předchozí.</span><span class="sxs-lookup"><span data-stu-id="ddce6-200">It creates one map per file in the output directory, and each map ensures that each key is less than or equal to the previous one.</span></span> <span data-ttu-id="ddce6-201">Mapování funkce generuje záznamy první a poslední klíčů každého souboru.</span><span class="sxs-lookup"><span data-stu-id="ddce6-201">The map function generates records of the first and last keys of each file.</span></span> <span data-ttu-id="ddce6-202">Snižte funkce zajišťuje, že první klíč souboru i vyšší než poslední klíč i-1 souboru.</span><span class="sxs-lookup"><span data-stu-id="ddce6-202">The reduce function ensures that the first key of file i is greater than the last key of file i-1.</span></span> <span data-ttu-id="ddce6-203">Jako výstup fázi snižte hlásit problémy s klíči, které jsou mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="ddce6-203">Any problems are reported as an output of the reduce phase, with the keys that are out of order.</span></span>

<span data-ttu-id="ddce6-204">Použijte následující kroky pro vygenerování dat, řazení a pak ověříte výstup:</span><span class="sxs-lookup"><span data-stu-id="ddce6-204">Use the following steps to generate data, sort, and then validate the output:</span></span>

1. <span data-ttu-id="ddce6-205">Generovat 10 GB dat, který je uložený na výchozí úložiště clusteru HDInsight na `/example/data/10GB-sort-input`:</span><span class="sxs-lookup"><span data-stu-id="ddce6-205">Generate 10 GB of data, which is stored to the HDInsight cluster's default storage at `/example/data/10GB-sort-input`:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    <span data-ttu-id="ddce6-206">`-Dmapred.map.tasks` Informuje Hadoop kolik mapy úlohy použít pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="ddce6-206">The `-Dmapred.map.tasks` tells Hadoop how many map tasks to use for this job.</span></span> <span data-ttu-id="ddce6-207">Poslední dva parametry pokyn úlohu vytvoření 10 GB dat a uložit ho v `/example/data/10GB-sort-input`.</span><span class="sxs-lookup"><span data-stu-id="ddce6-207">The final two parameters instruct the job to create 10 GB of data and to store it at `/example/data/10GB-sort-input`.</span></span>

2. <span data-ttu-id="ddce6-208">Použijte následující příkaz k řazení dat.:</span><span class="sxs-lookup"><span data-stu-id="ddce6-208">Use the following command to sort the data:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    <span data-ttu-id="ddce6-209">`-Dmapred.reduce.tasks` Informuje Hadoop, kolik snížit úlohy pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="ddce6-209">The `-Dmapred.reduce.tasks` tells Hadoop how many reduce tasks to use for the job.</span></span> <span data-ttu-id="ddce6-210">Poslední dva parametry jsou jenom vstupní a výstupní umístění pro data.</span><span class="sxs-lookup"><span data-stu-id="ddce6-210">The final two parameters are just the input and output locations for data.</span></span>

3. <span data-ttu-id="ddce6-211">Použijte následující postupy pro ověření dat generované řazení:</span><span class="sxs-lookup"><span data-stu-id="ddce6-211">Use the following to validate the data generated by the sort:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a><span data-ttu-id="ddce6-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ddce6-212">Next steps</span></span>

<span data-ttu-id="ddce6-213">V tomto článku jste se naučili ke spuštění ukázky, které jsou součástí clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="ddce6-213">From this article, you learned how to run the samples included with the Linux-based HDInsight clusters.</span></span> <span data-ttu-id="ddce6-214">Kurzech k používání Pig, Hive a MapReduce s HDInsight naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="ddce6-214">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see the following topics:</span></span>

* <span data-ttu-id="ddce6-215">[Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="ddce6-215">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="ddce6-216">[Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="ddce6-216">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="ddce6-217">[Používání nástroje MapReduce s Hadoop v HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="ddce6-217">[Use MapReduce with Hadoop on HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
