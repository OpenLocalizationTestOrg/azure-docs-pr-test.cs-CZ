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
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a><span data-ttu-id="8fe98-103">Používání nástroje MapReduce s Hadoop v HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="8fe98-103">Use MapReduce with Hadoop on HDInsight with SSH</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="8fe98-104">Zjistěte, jak toosubmit MapReduce úlohy z tooHDInsight připojení Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="8fe98-104">Learn how toosubmit MapReduce jobs from a Secure Shell (SSH) connection tooHDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="8fe98-105">Pokud jste již obeznámeni s používáním systémem Linux Hadoop servery, ale jsou nové tooHDInsight najdete [HDInsight se systémem Linux tipy](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="8fe98-105">If you are already familiar with using Linux-based Hadoop servers, but you are new tooHDInsight, see [Linux-based HDInsight tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="8fe98-106"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="8fe98-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="8fe98-107">Cluster HDInsight se systémem Linux (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="8fe98-107">A Linux-based HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="8fe98-108">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8fe98-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8fe98-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8fe98-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="8fe98-110">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="8fe98-110">An SSH client.</span></span> <span data-ttu-id="8fe98-111">Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="8fe98-111">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

## <span data-ttu-id="8fe98-112"><a id="ssh"></a>Připojení pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="8fe98-112"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="8fe98-113">Připojte toohello clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="8fe98-113">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="8fe98-114">Například následující příkaz hello připojuje tooa clusteru s názvem **myhdinsight**:</span><span class="sxs-lookup"><span data-stu-id="8fe98-114">For example, hello following command connects tooa cluster named **myhdinsight**:</span></span>

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="8fe98-115">**Pokud používáte klíč certifikátu pro ověřování SSH**, budete pravděpodobně toospecify hello umístění hello privátní klíč klientského systému, například:</span><span class="sxs-lookup"><span data-stu-id="8fe98-115">**If you use a certificate key for SSH authentication**, you may need toospecify hello location of hello private key on your client system, for example:</span></span>

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="8fe98-116">**Pokud použijete heslo pro ověřování SSH**, je nutné heslo hello tooprovide po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="8fe98-116">**If you use a password for SSH authentication**, you need tooprovide hello password when prompted.</span></span>

<span data-ttu-id="8fe98-117">Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8fe98-117">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="8fe98-118"><a id="hadoop"></a>Použít příkazy Hadoop</span><span class="sxs-lookup"><span data-stu-id="8fe98-118"><a id="hadoop"></a>Use Hadoop commands</span></span>

1. <span data-ttu-id="8fe98-119">Jakmile se připojené toohello clusteru HDInsight pomocí hello následující příkaz toostart úlohu MapReduce:</span><span class="sxs-lookup"><span data-stu-id="8fe98-119">After you are connected toohello HDInsight cluster, use hello following command toostart a MapReduce job:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    <span data-ttu-id="8fe98-120">Tento příkaz spustí hello `wordcount` třídy, která je součástí hello `hadoop-mapreduce-examples.jar` souboru.</span><span class="sxs-lookup"><span data-stu-id="8fe98-120">This command starts hello `wordcount` class, which is contained in hello `hadoop-mapreduce-examples.jar` file.</span></span> <span data-ttu-id="8fe98-121">Používá hello `/example/data/gutenberg/davinci.txt` dokumentu jako vstup a výstup je uloženo na `/example/data/WordCountOutput`.</span><span class="sxs-lookup"><span data-stu-id="8fe98-121">It uses hello `/example/data/gutenberg/davinci.txt` document as input, and output is stored at `/example/data/WordCountOutput`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8fe98-122">Další informace týkající se těchto dat. MapReduce úlohy a hello příklad najdete v tématu [použití MapReduce v Hadoop v HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="8fe98-122">For more information about this MapReduce job and hello example data, see [Use MapReduce in Hadoop on HDInsight](hdinsight-use-mapreduce.md).</span></span>

2. <span data-ttu-id="8fe98-123">Úloha Hello vysílá podrobnosti zpracovává a vrátí informace podobné toohello po dokončení úlohy hello následující text:</span><span class="sxs-lookup"><span data-stu-id="8fe98-123">hello job emits details as it processes, and it returns information similar toohello following text when hello job completes:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. <span data-ttu-id="8fe98-124">Po dokončení úlohy hello, použijte následující příkaz toolist hello výstupní soubory hello:</span><span class="sxs-lookup"><span data-stu-id="8fe98-124">When hello job completes, use hello following command toolist hello output files:</span></span>

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    <span data-ttu-id="8fe98-125">Tento příkaz zobrazí dva soubory, `_SUCCESS` a `part-r-00000`.</span><span class="sxs-lookup"><span data-stu-id="8fe98-125">This command display two files, `_SUCCESS` and `part-r-00000`.</span></span> <span data-ttu-id="8fe98-126">Hello `part-r-00000` soubor obsahuje hello výstup pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="8fe98-126">hello `part-r-00000` file contains hello output for this job.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8fe98-127">Některé úlohy MapReduce může rozdělit do několika hello výsledky **část r-###** soubory.</span><span class="sxs-lookup"><span data-stu-id="8fe98-127">Some MapReduce jobs may split hello results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="8fe98-128">Pokud ano, použít hello ### přípona tooindicate hello pořadí souborů hello.</span><span class="sxs-lookup"><span data-stu-id="8fe98-128">If so, use hello ##### suffix tooindicate hello order of hello files.</span></span>

4. <span data-ttu-id="8fe98-129">tooview hello výstup hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8fe98-129">tooview hello output, use hello following command:</span></span>

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    <span data-ttu-id="8fe98-130">Tento příkaz zobrazí seznam hello slova, které jsou obsaženy v hello **wasb://example/data/gutenberg/davinci.txt** souborové služby a hello stanovený počet jednotlivých slov došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="8fe98-130">This command displays a list of hello words that are contained in hello **wasb://example/data/gutenberg/davinci.txt** file and hello number of times each word occurred.</span></span> <span data-ttu-id="8fe98-131">Hello následující text je příkladem hello data obsažená v souboru hello:</span><span class="sxs-lookup"><span data-stu-id="8fe98-131">hello following text is an example of hello data that is contained in hello file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="8fe98-132"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="8fe98-132"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="8fe98-133">Jak můžete vidět, poskytují příkazy Hadoop toorun snadný způsob úloh MapReduce do clusteru HDInsight a pak výstup úlohy hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8fe98-133">As you can see, Hadoop commands provide an easy way toorun MapReduce jobs in an HDInsight cluster and then view hello job output.</span></span>

## <span data-ttu-id="8fe98-134"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="8fe98-134"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="8fe98-135">Obecné informace o úloh MapReduce v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8fe98-135">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="8fe98-136">Používání nástroje MapReduce systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="8fe98-136">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="8fe98-137">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8fe98-137">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="8fe98-138">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8fe98-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="8fe98-139">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8fe98-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
