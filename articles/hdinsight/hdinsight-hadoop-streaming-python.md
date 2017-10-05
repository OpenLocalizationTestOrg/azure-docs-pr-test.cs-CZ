---
title: "Vývoj Python streamování úloh MapReduce s HDInsight - Azure | Microsoft Docs"
description: "Naučte se používat jazyk Python ve streamování úloh MapReduce. Hadoop poskytuje streamování rozhraní API pro MapReduce pro zápis do jiných jazyků než Java."
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: b86605c49291a99f49c4b2841d46324cfd0db56d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a><span data-ttu-id="43be5-104">Vývoj Python streamování MapReduce programy pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="43be5-104">Develop Python streaming MapReduce programs for HDInsight</span></span>

<span data-ttu-id="43be5-105">Naučte se používat jazyk Python ve streamování MapReduce operations.</span><span class="sxs-lookup"><span data-stu-id="43be5-105">Learn how to use Python in streaming MapReduce operations.</span></span> <span data-ttu-id="43be5-106">Hadoop poskytuje streamování rozhraní API pro MapReduce, který umožňuje zapisovat mapy a omezit funkce v jiných jazyků než Java.</span><span class="sxs-lookup"><span data-stu-id="43be5-106">Hadoop provides a streaming API for MapReduce that enables you to write map and reduce functions in languages other than Java.</span></span> <span data-ttu-id="43be5-107">Kroky v tomto dokumentu implementovat mapy a snížit součásti v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="43be5-107">The steps in this document implement the Map and Reduce components in Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43be5-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="43be5-108">Prerequisites</span></span>

* <span data-ttu-id="43be5-109">Systémem Linux Hadoop v HDInsight clusteru</span><span class="sxs-lookup"><span data-stu-id="43be5-109">A Linux-based Hadoop on HDInsight cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="43be5-110">Kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="43be5-110">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="43be5-111">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="43be5-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="43be5-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="43be5-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="43be5-113">V textovém editoru</span><span class="sxs-lookup"><span data-stu-id="43be5-113">A text editor</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="43be5-114">Textový editor, musí používat LF jako ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="43be5-114">The text editor must use LF as the line ending.</span></span> <span data-ttu-id="43be5-115">Pomocí konec řádku Line FEED způsobuje chyby při spuštění úlohy MapReduce v clusterech HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="43be5-115">Using a line ending of CRLF causes errors when running the MapReduce job on Linux-based HDInsight clusters.</span></span>

* <span data-ttu-id="43be5-116">`ssh` a `scp` příkazy, nebo [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span><span class="sxs-lookup"><span data-stu-id="43be5-116">The `ssh` and `scp` commands, or [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span></span>

## <a name="word-count"></a><span data-ttu-id="43be5-117">Počet slov</span><span class="sxs-lookup"><span data-stu-id="43be5-117">Word count</span></span>

<span data-ttu-id="43be5-118">V tomto příkladu je, že počet základní slov implementované v python reduktorem a mapper.</span><span class="sxs-lookup"><span data-stu-id="43be5-118">This example is a basic word count implemented in a python a mapper and reducer.</span></span> <span data-ttu-id="43be5-119">Mapper věty dělí do jednotlivých slov a reduktorem slučuje slova a počty k vytvoření výstupu.</span><span class="sxs-lookup"><span data-stu-id="43be5-119">The mapper breaks sentences into individual words, and the reducer aggregates the words and counts to produce the output.</span></span>

<span data-ttu-id="43be5-120">Následující vývojový diagram znázorňuje, co se stane při mapy a snížit fáze.</span><span class="sxs-lookup"><span data-stu-id="43be5-120">The following flowchart illustrates what happens during the map and reduce phases.</span></span>

![Obrázek procesu mapreduce](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a><span data-ttu-id="43be5-122">Streamování MapReduce</span><span class="sxs-lookup"><span data-stu-id="43be5-122">Streaming MapReduce</span></span>

<span data-ttu-id="43be5-123">Hadoop umožňuje zadat soubor, který obsahuje mapy a snížit logiky, která se používá v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="43be5-123">Hadoop allows you to specify a file that contains the map and reduce logic that is used by a job.</span></span> <span data-ttu-id="43be5-124">Požadavky na konkrétní mapy a snížit logiky jsou:</span><span class="sxs-lookup"><span data-stu-id="43be5-124">The specific requirements for the map and reduce logic are:</span></span>

* <span data-ttu-id="43be5-125">**Vstupní**: mapy a snížit součásti musí číst vstupní data z STDIN.</span><span class="sxs-lookup"><span data-stu-id="43be5-125">**Input**: The map and reduce components must read input data from STDIN.</span></span>
* <span data-ttu-id="43be5-126">**Výstup**: mapy a snížit součásti musí zapsat do STDOUT výstupní data.</span><span class="sxs-lookup"><span data-stu-id="43be5-126">**Output**: The map and reduce components must write output data to STDOUT.</span></span>
* <span data-ttu-id="43be5-127">**Formát dat**: data využívat a vytváří musí být dvojice klíč/hodnota, oddělených tabulátorem.</span><span class="sxs-lookup"><span data-stu-id="43be5-127">**Data format**: The data consumed and produced must be a key/value pair, separated by a tab character.</span></span>

<span data-ttu-id="43be5-128">Python můžete snadno zpracování těchto požadavků pomocí `sys` modulu číst z stdin – a pomocí `print` tisknout na STDOUT.</span><span class="sxs-lookup"><span data-stu-id="43be5-128">Python can easily handle these requirements by using the `sys` module to read from STDIN and using `print` to print to STDOUT.</span></span> <span data-ttu-id="43be5-129">Zbývající úloh je jednoduše formátování dat pomocí na kartě (`\t`) znak mezi klíčem a hodnotou.</span><span class="sxs-lookup"><span data-stu-id="43be5-129">The remaining task is simply formatting the data with a tab (`\t`) character between the key and value.</span></span>

## <a name="create-the-mapper-and-reducer"></a><span data-ttu-id="43be5-130">Vytvoření mapper a reduktorem</span><span class="sxs-lookup"><span data-stu-id="43be5-130">Create the mapper and reducer</span></span>

1. <span data-ttu-id="43be5-131">Vytvořte soubor s názvem `mapper.py` a použít následující kód jako obsah:</span><span class="sxs-lookup"><span data-stu-id="43be5-131">Create a file named `mapper.py` and use the following code as the content:</span></span>

   ```python
   #!/usr/bin/env python

   # Use the sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read the data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write to STDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. <span data-ttu-id="43be5-132">Vytvořte soubor s názvem **reducer.py** a použít následující kód jako obsah:</span><span class="sxs-lookup"><span data-stu-id="43be5-132">Create a file named **reducer.py** and use the following code as the content:</span></span>

   ```python
   #!/usr/bin/env python

   # import modules
   from itertools import groupby
   from operator import itemgetter
   import sys

   # 'file' in this case is STDIN
   def read_mapper_output(file, separator='\t'):
       # Go through each line
       for line in file:
           # Strip out the separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read the data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using the word as the key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull the count(s) for the word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write to stdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a><span data-ttu-id="43be5-133">Spuštění pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="43be5-133">Run using PowerShell</span></span>

<span data-ttu-id="43be5-134">K zajištění, že soubory mají právo konce řádků, použijte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="43be5-134">To ensure that your files have the right line endings, use the following PowerShell script:</span></span>

<span data-ttu-id="43be5-135">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]</span><span class="sxs-lookup"><span data-stu-id="43be5-135">[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]</span></span>

<span data-ttu-id="43be5-136">Pomocí následujícího skriptu prostředí PowerShell k odesílání souborů, spusťte úlohu a zobrazte výstup:</span><span class="sxs-lookup"><span data-stu-id="43be5-136">Use the following PowerShell script to upload the files, run the job, and view the output:</span></span>

<span data-ttu-id="43be5-137">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]</span><span class="sxs-lookup"><span data-stu-id="43be5-137">[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]</span></span>

## <a name="run-from-an-ssh-session"></a><span data-ttu-id="43be5-138">Spuštění z relace SSH</span><span class="sxs-lookup"><span data-stu-id="43be5-138">Run from an SSH session</span></span>

1. <span data-ttu-id="43be5-139">Z vývojového prostředí, ve stejném adresáři jako `mapper.py` a `reducer.py` soubory, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43be5-139">From your development environment, in the same directory as `mapper.py` and `reducer.py` files, use the following command:</span></span>

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="43be5-140">Nahraďte `username` s uživatelským jménem SSH pro váš cluster a `clustername` s názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="43be5-140">Replace `username` with the SSH user name for your cluster, and `clustername` with the name of your cluster.</span></span>

    <span data-ttu-id="43be5-141">Tento příkaz zkopíruje soubory z místního systému k hlavnímu uzlu.</span><span class="sxs-lookup"><span data-stu-id="43be5-141">This command copies the files from the local system to the head node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="43be5-142">Pokud jste použili heslo k zabezpečení účtu SSH, zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="43be5-142">If you used a password to secure your SSH account, you are prompted for the password.</span></span> <span data-ttu-id="43be5-143">Pokud jste použili klíč SSH, budete možná muset použít `-i` parametr a cestu k privátnímu klíči.</span><span class="sxs-lookup"><span data-stu-id="43be5-143">If you used an SSH key, you may have to use the `-i` parameter and the path to the private key.</span></span> <span data-ttu-id="43be5-144">Například, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="43be5-144">For example, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="43be5-145">Připojte se ke clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="43be5-145">Connect to the cluster by using SSH:</span></span>

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    <span data-ttu-id="43be5-146">Další informace o najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="43be5-146">For more information on, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="43be5-147">K zajištění, že mapper.py a reducer.py konců správné čar, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="43be5-147">To ensure the mapper.py and reducer.py have the correct line endings, use the following commands:</span></span>

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. <span data-ttu-id="43be5-148">Použijte následující příkaz pro spuštění úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="43be5-148">Use the following command to start the MapReduce job.</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    <span data-ttu-id="43be5-149">Tento příkaz má následující části:</span><span class="sxs-lookup"><span data-stu-id="43be5-149">This command has the following parts:</span></span>

   * <span data-ttu-id="43be5-150">**hadoop streaming.jar**: používá při provádění operací streamování MapReduce.</span><span class="sxs-lookup"><span data-stu-id="43be5-150">**hadoop-streaming.jar**: Used when performing streaming MapReduce operations.</span></span> <span data-ttu-id="43be5-151">Je rozhraní Hadoop externí MapReduce kódem, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="43be5-151">It interfaces Hadoop with the external MapReduce code you provide.</span></span>

   * <span data-ttu-id="43be5-152">**-soubory**: Přidá zadané soubory do úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="43be5-152">**-files**: Adds the specified files to the MapReduce job.</span></span>

   * <span data-ttu-id="43be5-153">**-mapper**: informuje Hadoop, který soubor má použít jako mapper.</span><span class="sxs-lookup"><span data-stu-id="43be5-153">**-mapper**: Tells Hadoop which file to use as the mapper.</span></span>

   * <span data-ttu-id="43be5-154">**-reduktorem**: informuje Hadoop, který soubor má použít jako reduktorem.</span><span class="sxs-lookup"><span data-stu-id="43be5-154">**-reducer**: Tells Hadoop which file to use as the reducer.</span></span>

   * <span data-ttu-id="43be5-155">**-vstupní**: slova vstupního souboru, který jsme měli počítat z.</span><span class="sxs-lookup"><span data-stu-id="43be5-155">**-input**: The input file that we should count words from.</span></span>

   * <span data-ttu-id="43be5-156">**-výstupu**: adresář, který je výstup zapsán do.</span><span class="sxs-lookup"><span data-stu-id="43be5-156">**-output**: The directory that the output is written to.</span></span>

    <span data-ttu-id="43be5-157">Jak funguje úlohu MapReduce, proces se zobrazí jako procenta.</span><span class="sxs-lookup"><span data-stu-id="43be5-157">As the MapReduce job works, the process is displayed as percentages.</span></span>

        <span data-ttu-id="43be5-158">15/02/05 19:01:04 informace o mapreduce. Úloha: % 0 mapy snížit 0 % 15/02/05 19:01:16 informace o mapreduce. Úloha: 100 % mapy snížit 0 % 15/02/05 19:01:27 informace o mapreduce. Úloha: 100 % mapy snížit 100 %</span><span class="sxs-lookup"><span data-stu-id="43be5-158">15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%</span></span>


5. <span data-ttu-id="43be5-159">Chcete-li zobrazit výstup, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43be5-159">To view the output, use the following command:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="43be5-160">Tento příkaz zobrazí seznam slova a jak často slovo došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="43be5-160">This command displays a list of words and how many times the word occurred.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43be5-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43be5-161">Next steps</span></span>

<span data-ttu-id="43be5-162">Teď, když jste se naučili použití úlohy streamování MapRedcue s HDInsight, pomocí následujících odkazů a prozkoumejte další způsoby, jak pracovat s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="43be5-162">Now that you have learned how to use streaming MapRedcue jobs with HDInsight, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* [<span data-ttu-id="43be5-163">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="43be5-163">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="43be5-164">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="43be5-164">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="43be5-165">Použití úloh MapReduce se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="43be5-165">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
