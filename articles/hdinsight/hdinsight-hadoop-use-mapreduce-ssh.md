---
title: "Připojení MapReduce a SSH s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití SSH ke spuštění úloh MapReduce pomocí Hadoop v HDInsight."
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
ms.openlocfilehash: eaf6278f97cd5ddd7e049ff4745181f39d7949a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a><span data-ttu-id="f838f-103">Používání nástroje MapReduce s Hadoop v HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="f838f-103">Use MapReduce with Hadoop on HDInsight with SSH</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="f838f-104">Zjistěte, jak k odesílání úloh MapReduce připojení Secure Shell (SSH) do HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f838f-104">Learn how to submit MapReduce jobs from a Secure Shell (SSH) connection to HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="f838f-105">Pokud jste již obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou pro vás nové do HDInsight, najdete v části [HDInsight se systémem Linux tipy](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="f838f-105">If you are already familiar with using Linux-based Hadoop servers, but you are new to HDInsight, see [Linux-based HDInsight tips](hdinsight-hadoop-linux-information.md).</span></span>

## <span data-ttu-id="f838f-106"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="f838f-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="f838f-107">Cluster HDInsight se systémem Linux (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="f838f-107">A Linux-based HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="f838f-108">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="f838f-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f838f-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f838f-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="f838f-110">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="f838f-110">An SSH client.</span></span> <span data-ttu-id="f838f-111">Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="f838f-111">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

## <span data-ttu-id="f838f-112"><a id="ssh"></a>Připojení pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="f838f-112"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="f838f-113">Připojte se ke clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="f838f-113">Connect to the cluster using SSH.</span></span> <span data-ttu-id="f838f-114">Například následující příkaz se připojí ke clusteru s názvem **myhdinsight**:</span><span class="sxs-lookup"><span data-stu-id="f838f-114">For example, the following command connects to a cluster named **myhdinsight**:</span></span>

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="f838f-115">**Pokud používáte klíč certifikátu pro ověřování SSH**, budete možná muset zadat umístění privátní klíč klientského systému, například:</span><span class="sxs-lookup"><span data-stu-id="f838f-115">**If you use a certificate key for SSH authentication**, you may need to specify the location of the private key on your client system, for example:</span></span>

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

<span data-ttu-id="f838f-116">**Pokud použijete heslo pro ověřování SSH**, budete muset zadat heslo po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="f838f-116">**If you use a password for SSH authentication**, you need to provide the password when prompted.</span></span>

<span data-ttu-id="f838f-117">Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="f838f-117">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="f838f-118"><a id="hadoop"></a>Použít příkazy Hadoop</span><span class="sxs-lookup"><span data-stu-id="f838f-118"><a id="hadoop"></a>Use Hadoop commands</span></span>

1. <span data-ttu-id="f838f-119">Po připojení ke clusteru HDInsight, použijte následující příkaz spusťte úlohu MapReduce:</span><span class="sxs-lookup"><span data-stu-id="f838f-119">After you are connected to the HDInsight cluster, use the following command to start a MapReduce job:</span></span>

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    <span data-ttu-id="f838f-120">Tento příkaz spustí `wordcount` třídy, která je součástí `hadoop-mapreduce-examples.jar` souboru.</span><span class="sxs-lookup"><span data-stu-id="f838f-120">This command starts the `wordcount` class, which is contained in the `hadoop-mapreduce-examples.jar` file.</span></span> <span data-ttu-id="f838f-121">Použije `/example/data/gutenberg/davinci.txt` dokumentu jako vstup a výstup je uloženo na `/example/data/WordCountOutput`.</span><span class="sxs-lookup"><span data-stu-id="f838f-121">It uses the `/example/data/gutenberg/davinci.txt` document as input, and output is stored at `/example/data/WordCountOutput`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f838f-122">Další informace o této úlohy MapReduce a data příklad najdete v tématu [použití MapReduce v Hadoop v HDInsight](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="f838f-122">For more information about this MapReduce job and the example data, see [Use MapReduce in Hadoop on HDInsight](hdinsight-use-mapreduce.md).</span></span>

2. <span data-ttu-id="f838f-123">Úloha vysílá podrobnosti zpracovává a vrátí informace podobná následující text po dokončení úlohy:</span><span class="sxs-lookup"><span data-stu-id="f838f-123">The job emits details as it processes, and it returns information similar to the following text when the job completes:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. <span data-ttu-id="f838f-124">Po dokončení úlohy, použijte následující příkaz k zobrazení seznamu výstupní soubory:</span><span class="sxs-lookup"><span data-stu-id="f838f-124">When the job completes, use the following command to list the output files:</span></span>

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    <span data-ttu-id="f838f-125">Tento příkaz zobrazí dva soubory, `_SUCCESS` a `part-r-00000`.</span><span class="sxs-lookup"><span data-stu-id="f838f-125">This command display two files, `_SUCCESS` and `part-r-00000`.</span></span> <span data-ttu-id="f838f-126">`part-r-00000` Soubor obsahuje výstup pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="f838f-126">The `part-r-00000` file contains the output for this job.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f838f-127">Některé úlohy MapReduce může rozdělit do několika výsledky **část r-###** soubory.</span><span class="sxs-lookup"><span data-stu-id="f838f-127">Some MapReduce jobs may split the results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="f838f-128">Pokud ano, použít ### příponu označte pořadí souborů.</span><span class="sxs-lookup"><span data-stu-id="f838f-128">If so, use the ##### suffix to indicate the order of the files.</span></span>

4. <span data-ttu-id="f838f-129">Chcete-li zobrazit výstup, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f838f-129">To view the output, use the following command:</span></span>

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    <span data-ttu-id="f838f-130">Tento příkaz zobrazí seznam slova, která jsou součástí **wasb://example/data/gutenberg/davinci.txt** souboru a počet jednotlivých slov došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="f838f-130">This command displays a list of the words that are contained in the **wasb://example/data/gutenberg/davinci.txt** file and the number of times each word occurred.</span></span> <span data-ttu-id="f838f-131">Tento text je příklad dat, která je obsažená v souboru:</span><span class="sxs-lookup"><span data-stu-id="f838f-131">The following text is an example of the data that is contained in the file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="f838f-132"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="f838f-132"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="f838f-133">Jak vidíte, poskytují příkazy Hadoop snadný způsob, jak spouštět úlohy MapReduce v clusteru služby HDInsight a pak zobrazit výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="f838f-133">As you can see, Hadoop commands provide an easy way to run MapReduce jobs in an HDInsight cluster and then view the job output.</span></span>

## <span data-ttu-id="f838f-134"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="f838f-134"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="f838f-135">Obecné informace o úloh MapReduce v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f838f-135">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="f838f-136">Používání nástroje MapReduce systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="f838f-136">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="f838f-137">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f838f-137">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="f838f-138">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f838f-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="f838f-139">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f838f-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
