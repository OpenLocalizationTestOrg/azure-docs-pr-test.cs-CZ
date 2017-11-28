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
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="d4832-103">Používání nástroje MapReduce v Hadoop v HDInsight pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="d4832-103">Use MapReduce in Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="d4832-104">V tomto článku se dozvíte, jak clusteru tooconnect tooa Hadoop v HDInsight pomocí vzdálené plochy a potom spusťte úloh MapReduce pomocí příkazu Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="d4832-104">In this article, you will learn how tooconnect tooa Hadoop on HDInsight cluster by using Remote Desktop and then run MapReduce jobs by using hello Hadoop command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d4832-105">Vzdálená plocha je dostupná pouze na clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="d4832-105">Remote Desktop is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="d4832-106">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d4832-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d4832-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d4832-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="d4832-108">HDInsight 3.4 nebo větší, projděte si téma [používání MapReduce s SSH](hdinsight-hadoop-use-mapreduce-ssh.md) informace o připojení toohello HDInsight cluster a spuštění úloh MapReduce.</span><span class="sxs-lookup"><span data-stu-id="d4832-108">For HDInsight 3.4 or greater, see [Use MapReduce with SSH](hdinsight-hadoop-use-mapreduce-ssh.md) for information on connecting toohello HDInsight cluster and running MapReduce jobs.</span></span>

## <span data-ttu-id="d4832-109"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="d4832-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="d4832-110">toocomplete hello kroky v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="d4832-110">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="d4832-111">Cluster HDInsight se systémem Windows (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="d4832-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="d4832-112">Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7</span><span class="sxs-lookup"><span data-stu-id="d4832-112">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="d4832-113"><a id="connect"></a>Připojit pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="d4832-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="d4832-114">Povolení vzdálené plochy pro hello HDInsight cluster a pak připojit tooit podle následujících pokynů hello [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="d4832-114">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="d4832-115"><a id="hadoop"></a>Použijte příkaz Hadoop hello</span><span class="sxs-lookup"><span data-stu-id="d4832-115"><a id="hadoop"></a>Use hello Hadoop command</span></span>
<span data-ttu-id="d4832-116">Pokud jste desktop připojený toohello pro hello HDInsight cluster, použijte následující kroky toorun úlohu MapReduce pomocí příkazu Hadoop hello hello:</span><span class="sxs-lookup"><span data-stu-id="d4832-116">When you are connected toohello desktop for hello HDInsight cluster, use hello following steps toorun a MapReduce job by using hello Hadoop command:</span></span>

1. <span data-ttu-id="d4832-117">Z plochy hello HDInsight, spusťte hello **Hadoop příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="d4832-117">From hello HDInsight desktop, start hello **Hadoop Command Line**.</span></span> <span data-ttu-id="d4832-118">Otevře se nové příkazový řádek v hello **c:\apps\dist\hadoop-&lt;číslo verze >** adresáře.</span><span class="sxs-lookup"><span data-stu-id="d4832-118">This opens a new command prompt in hello **c:\apps\dist\hadoop-&lt;version number>** directory.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d4832-119">číslo verze Hello změní, když se aktualizuje Hadoop.</span><span class="sxs-lookup"><span data-stu-id="d4832-119">hello version number changes as Hadoop is updated.</span></span> <span data-ttu-id="d4832-120">Hello **HADOOP_HOME** proměnnou prostředí může být cesta použité toofind hello.</span><span class="sxs-lookup"><span data-stu-id="d4832-120">hello **HADOOP_HOME** environment variable can be used toofind hello path.</span></span> <span data-ttu-id="d4832-121">Například `cd %HADOOP_HOME%` změny adresáře toohello Hadoop adresáři, aniž byste si číslo verze tooknow hello.</span><span class="sxs-lookup"><span data-stu-id="d4832-121">For example, `cd %HADOOP_HOME%` changes directories toohello Hadoop directory, without requiring you tooknow hello version number.</span></span>
   >
   >
2. <span data-ttu-id="d4832-122">toouse hello **Hadoop** příkaz toorun úlohu MapReduce příklad, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d4832-122">toouse hello **Hadoop** command toorun an example MapReduce job, use hello following command:</span></span>

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    <span data-ttu-id="d4832-123">Tím se spustí hello **wordcount** třídy, která je součástí hello **hadoop-mapreduce-examples.jar** soubor v aktuálním adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="d4832-123">This starts hello **wordcount** class, which is contained in hello **hadoop-mapreduce-examples.jar** file in hello current directory.</span></span> <span data-ttu-id="d4832-124">Jako vstup, používá hello **wasb://example/data/gutenberg/davinci.txt** dokumentu a výstup je uložený v: **wasb: / / / Příklad/data/WordCountOutput**.</span><span class="sxs-lookup"><span data-stu-id="d4832-124">As input, it uses hello **wasb://example/data/gutenberg/davinci.txt** document, and output is stored at: **wasb:///example/data/WordCountOutput**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d4832-125">Další informace týkající se těchto dat. MapReduce úlohy a hello příklad najdete v tématu <a href="hdinsight-use-mapreduce.md">použití MapReduce v HDInsight Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="d4832-125">for more information about this MapReduce job and hello example data, see <a href="hdinsight-use-mapreduce.md">Use MapReduce in HDInsight Hadoop</a>.</span></span>
   >
   >
3. <span data-ttu-id="d4832-126">Úloha Hello vysílá podrobnosti, jako je zpracován a vrátí informace podobné toohello následující po dokončení úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="d4832-126">hello job emits details as it is processed, and it returns information similar toohello following when hello job is complete:</span></span>

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. <span data-ttu-id="d4832-127">Po dokončení úlohy hello, použijte následující příkaz toolist hello výstupní soubory uložené v hello **wasb://example/data/WordCountOutput**:</span><span class="sxs-lookup"><span data-stu-id="d4832-127">When hello job is complete, use hello following command toolist hello output files stored at **wasb://example/data/WordCountOutput**:</span></span>

        hadoop fs -ls wasb:///example/data/WordCountOutput

    <span data-ttu-id="d4832-128">To by měl zobrazit dva soubory, **_SUCCESS** a **část r-00000**.</span><span class="sxs-lookup"><span data-stu-id="d4832-128">This should display two files, **_SUCCESS** and **part-r-00000**.</span></span> <span data-ttu-id="d4832-129">Hello **část r-00000** soubor obsahuje hello výstup pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="d4832-129">hello **part-r-00000** file contains hello output for this job.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d4832-130">Některé úlohy MapReduce může rozdělit do několika hello výsledky **část r-###** soubory.</span><span class="sxs-lookup"><span data-stu-id="d4832-130">Some MapReduce jobs may split hello results across multiple **part-r-#####** files.</span></span> <span data-ttu-id="d4832-131">Pokud ano, použít hello ### přípona tooindicate hello pořadí souborů hello.</span><span class="sxs-lookup"><span data-stu-id="d4832-131">If so, use hello ##### suffix tooindicate hello order of hello files.</span></span>
   >
   >
5. <span data-ttu-id="d4832-132">tooview hello výstup hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d4832-132">tooview hello output, use hello following command:</span></span>

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    <span data-ttu-id="d4832-133">Zobrazí se seznam hello slova, které jsou obsaženy v hello **wasb://example/data/gutenberg/davinci.txt** souboru, společně s hello počet jednotlivých slov došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="d4832-133">This displays a list of hello words that are contained in hello **wasb://example/data/gutenberg/davinci.txt** file, along with hello number of times each word occured.</span></span> <span data-ttu-id="d4832-134">Hello tady je příklad hello dat, který bude obsažen v souboru hello:</span><span class="sxs-lookup"><span data-stu-id="d4832-134">hello following is an example of hello data that will be contained in hello file:</span></span>

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <span data-ttu-id="d4832-135"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="d4832-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="d4832-136">Jak vidíte, bude hello příkaz Hadoop na clusteru HDInsight a pak výstup úlohy hello zobrazení poskytuje snadný způsob toorun úloh MapReduce.</span><span class="sxs-lookup"><span data-stu-id="d4832-136">As you can see, hello Hadoop command provides an easy way toorun MapReduce jobs on an HDInsight cluster and then view hello job output.</span></span>

## <span data-ttu-id="d4832-137"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4832-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="d4832-138">Obecné informace o úloh MapReduce v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d4832-138">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="d4832-139">Používání nástroje MapReduce systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="d4832-139">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="d4832-140">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d4832-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="d4832-141">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d4832-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="d4832-142">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d4832-142">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
