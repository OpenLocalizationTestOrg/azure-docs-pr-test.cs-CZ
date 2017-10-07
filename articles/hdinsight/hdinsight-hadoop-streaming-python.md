---
title: "aaaDevelop úlohy Python streamování MapReduce s HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Python ve streamování úloh MapReduce. Hadoop poskytuje streamování rozhraní API pro MapReduce pro zápis do jiných jazyků než Java."
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
ms.openlocfilehash: a6ae3ba650b665ecc5839a4ddf5282f8ccfb6bd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a><span data-ttu-id="ab1ed-104">Vývoj Python streamování MapReduce programy pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab1ed-104">Develop Python streaming MapReduce programs for HDInsight</span></span>

<span data-ttu-id="ab1ed-105">Zjistěte, jak toouse Python ve streamování MapReduce operations.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-105">Learn how toouse Python in streaming MapReduce operations.</span></span> <span data-ttu-id="ab1ed-106">Hadoop poskytuje streamování rozhraní API pro MapReduce, která umožní toowrite mapy a omezit funkce v jiných jazyků než Java.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-106">Hadoop provides a streaming API for MapReduce that enables you toowrite map and reduce functions in languages other than Java.</span></span> <span data-ttu-id="ab1ed-107">Hello kroky v tomto dokumentu implementovat hello mapy a snížit součásti v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-107">hello steps in this document implement hello Map and Reduce components in Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab1ed-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ab1ed-108">Prerequisites</span></span>

* <span data-ttu-id="ab1ed-109">Systémem Linux Hadoop v HDInsight clusteru</span><span class="sxs-lookup"><span data-stu-id="ab1ed-109">A Linux-based Hadoop on HDInsight cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="ab1ed-110">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-110">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="ab1ed-111">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ab1ed-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ab1ed-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="ab1ed-113">V textovém editoru</span><span class="sxs-lookup"><span data-stu-id="ab1ed-113">A text editor</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="ab1ed-114">Hello textového editoru musí používat LF jako hello ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-114">hello text editor must use LF as hello line ending.</span></span> <span data-ttu-id="ab1ed-115">Pomocí konec řádku Line FEED způsobuje chyby při spuštění úlohy MapReduce hello v clusterech HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-115">Using a line ending of CRLF causes errors when running hello MapReduce job on Linux-based HDInsight clusters.</span></span>

* <span data-ttu-id="ab1ed-116">Hello `ssh` a `scp` příkazy, nebo [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span><span class="sxs-lookup"><span data-stu-id="ab1ed-116">hello `ssh` and `scp` commands, or [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)</span></span>

## <a name="word-count"></a><span data-ttu-id="ab1ed-117">Počet slov</span><span class="sxs-lookup"><span data-stu-id="ab1ed-117">Word count</span></span>

<span data-ttu-id="ab1ed-118">V tomto příkladu je, že počet základní slov implementované v python reduktorem a mapper.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-118">This example is a basic word count implemented in a python a mapper and reducer.</span></span> <span data-ttu-id="ab1ed-119">Mapovač Hello věty dělí do jednotlivých slov a hello reduktorem slučuje hello slova a počty tooproduce hello výstup.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-119">hello mapper breaks sentences into individual words, and hello reducer aggregates hello words and counts tooproduce hello output.</span></span>

<span data-ttu-id="ab1ed-120">Hello následující vývojový diagram znázorňuje, co se stane při hello mapy a snížit fáze.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-120">hello following flowchart illustrates what happens during hello map and reduce phases.</span></span>

![Obrázek procesu mapreduce hello](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a><span data-ttu-id="ab1ed-122">Streamování MapReduce</span><span class="sxs-lookup"><span data-stu-id="ab1ed-122">Streaming MapReduce</span></span>

<span data-ttu-id="ab1ed-123">Hadoop vám umožní toospecify soubor, který obsahuje hello mapy a snížit logiky, která se používá v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-123">Hadoop allows you toospecify a file that contains hello map and reduce logic that is used by a job.</span></span> <span data-ttu-id="ab1ed-124">zvláštní požadavky Hello hello mapování a snížit logiky jsou:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-124">hello specific requirements for hello map and reduce logic are:</span></span>

* <span data-ttu-id="ab1ed-125">**Vstupní**: hello mapy a snížit součásti musí číst vstupní data z STDIN.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-125">**Input**: hello map and reduce components must read input data from STDIN.</span></span>
* <span data-ttu-id="ab1ed-126">**Výstup**: hello mapy a snížit součásti musíte napsat tooSTDOUT výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-126">**Output**: hello map and reduce components must write output data tooSTDOUT.</span></span>
* <span data-ttu-id="ab1ed-127">**Formát dat**: data hello spotřebované a vytváří musí být dvojice klíč/hodnota, oddělených tabulátorem.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-127">**Data format**: hello data consumed and produced must be a key/value pair, separated by a tab character.</span></span>

<span data-ttu-id="ab1ed-128">Python lze snadno zpracovat tyto požadavky pomocí hello `sys` modulu tooread z stdin – a pomocí `print` tooprint tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-128">Python can easily handle these requirements by using hello `sys` module tooread from STDIN and using `print` tooprint tooSTDOUT.</span></span> <span data-ttu-id="ab1ed-129">Hello zbývajících úloh je jednoduše formátování hello dat s na kartě (`\t`) znak mezi hello klíč a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-129">hello remaining task is simply formatting hello data with a tab (`\t`) character between hello key and value.</span></span>

## <a name="create-hello-mapper-and-reducer"></a><span data-ttu-id="ab1ed-130">Vytvoření hello mapper a reduktorem</span><span class="sxs-lookup"><span data-stu-id="ab1ed-130">Create hello mapper and reducer</span></span>

1. <span data-ttu-id="ab1ed-131">Vytvořte soubor s názvem `mapper.py` a hello použijte následující kód jako obsah hello:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-131">Create a file named `mapper.py` and use hello following code as hello content:</span></span>

   ```python
   #!/usr/bin/env python

   # Use hello sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read hello data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write tooSTDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. <span data-ttu-id="ab1ed-132">Vytvořte soubor s názvem **reducer.py** a hello použijte následující kód jako obsah hello:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-132">Create a file named **reducer.py** and use hello following code as hello content:</span></span>

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
           # Strip out hello separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read hello data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using hello word as hello key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull hello count(s) for hello word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write toostdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a><span data-ttu-id="ab1ed-133">Spuštění pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ab1ed-133">Run using PowerShell</span></span>

<span data-ttu-id="ab1ed-134">tooensure splnit vaše soubory hello pravého konce řádků, hello použijte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-134">tooensure that your files have hello right line endings, use hello following PowerShell script:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

<span data-ttu-id="ab1ed-135">Pomocí hello následující soubory hello tooupload skriptu prostředí PowerShell, spusťte úlohu hello a zobrazit výstup hello:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-135">Use hello following PowerShell script tooupload hello files, run hello job, and view hello output:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a><span data-ttu-id="ab1ed-136">Spuštění z relace SSH</span><span class="sxs-lookup"><span data-stu-id="ab1ed-136">Run from an SSH session</span></span>

1. <span data-ttu-id="ab1ed-137">Z vývojového prostředí v hello stejný adresář jako `mapper.py` a `reducer.py` soubory, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-137">From your development environment, in hello same directory as `mapper.py` and `reducer.py` files, use hello following command:</span></span>

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="ab1ed-138">Nahraďte `username` hello uživatelským jménem SSH pro váš cluster a `clustername` s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-138">Replace `username` with hello SSH user name for your cluster, and `clustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="ab1ed-139">Tento příkaz zkopíruje soubory hello z hlavního uzlu toohello hello místní systém.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-139">This command copies hello files from hello local system toohello head node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ab1ed-140">Pokud jste použili toosecure heslo účtu SSH, budete vyzváni k hello heslo.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-140">If you used a password toosecure your SSH account, you are prompted for hello password.</span></span> <span data-ttu-id="ab1ed-141">Pokud jste použili klíč SSH, můžete mít toouse hello `-i` parametr a hello cesta toohello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-141">If you used an SSH key, you may have toouse hello `-i` parameter and hello path toohello private key.</span></span> <span data-ttu-id="ab1ed-142">Například, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-142">For example, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="ab1ed-143">Připojte toohello clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-143">Connect toohello cluster by using SSH:</span></span>

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    <span data-ttu-id="ab1ed-144">Další informace o najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ab1ed-144">For more information on, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="ab1ed-145">tooensure hello mapper.py a reducer.py mají hello opravte konce řádků, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-145">tooensure hello mapper.py and reducer.py have hello correct line endings, use hello following commands:</span></span>

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. <span data-ttu-id="ab1ed-146">Použijte následující příkaz toostart hello MapReduce úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-146">Use hello following command toostart hello MapReduce job.</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    <span data-ttu-id="ab1ed-147">Tento příkaz má hello následující části:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-147">This command has hello following parts:</span></span>

   * <span data-ttu-id="ab1ed-148">**hadoop streaming.jar**: používá při provádění operací streamování MapReduce.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-148">**hadoop-streaming.jar**: Used when performing streaming MapReduce operations.</span></span> <span data-ttu-id="ab1ed-149">Je rozhraní Hadoop s hello externí MapReduce kód, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-149">It interfaces Hadoop with hello external MapReduce code you provide.</span></span>

   * <span data-ttu-id="ab1ed-150">**-soubory**: Přidá hello zadaný úlohu MapReduce toohello soubory.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-150">**-files**: Adds hello specified files toohello MapReduce job.</span></span>

   * <span data-ttu-id="ab1ed-151">**-mapper**: informuje Hadoop, který toouse souborů, jako jsou třeba hello mapper.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-151">**-mapper**: Tells Hadoop which file toouse as hello mapper.</span></span>

   * <span data-ttu-id="ab1ed-152">**-reduktorem**: informuje Hadoop, který toouse souborů, jako jsou třeba hello reduktorem.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-152">**-reducer**: Tells Hadoop which file toouse as hello reducer.</span></span>

   * <span data-ttu-id="ab1ed-153">**-vstupní**: slova hello vstupního souboru, který jsme měli počítat z.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-153">**-input**: hello input file that we should count words from.</span></span>

   * <span data-ttu-id="ab1ed-154">**-výstupu**: hello adresář, který hello výstup je zapsán do.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-154">**-output**: hello directory that hello output is written to.</span></span>

    <span data-ttu-id="ab1ed-155">Jak funguje hello úlohu MapReduce, hello procesu se zobrazí jako procenta.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-155">As hello MapReduce job works, hello process is displayed as percentages.</span></span>

        <span data-ttu-id="ab1ed-156">15/02/05 19:01:04 informace o mapreduce. Úloha: % 0 mapy snížit 0 % 15/02/05 19:01:16 informace o mapreduce. Úloha: 100 % mapy snížit 0 % 15/02/05 19:01:27 informace o mapreduce. Úloha: 100 % mapy snížit 100 %</span><span class="sxs-lookup"><span data-stu-id="ab1ed-156">15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%</span></span>


5. <span data-ttu-id="ab1ed-157">tooview hello výstup hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab1ed-157">tooview hello output, use hello following command:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="ab1ed-158">Tento příkaz zobrazí seznam slova a jak často hello word došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-158">This command displays a list of words and how many times hello word occurred.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab1ed-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab1ed-159">Next steps</span></span>

<span data-ttu-id="ab1ed-160">Teď, když jste se naučili, jak toouse streamování MapRedcue úlohy s HDInsight, použijte následující odkazy tooexplore hello jiné způsoby toowork s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ab1ed-160">Now that you have learned how toouse streaming MapRedcue jobs with HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* [<span data-ttu-id="ab1ed-161">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab1ed-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ab1ed-162">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab1ed-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ab1ed-163">Použití úloh MapReduce se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab1ed-163">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
