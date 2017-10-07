---
title: "aaaUse Hadoop Pig pomocí protokolu SSH v clusteru HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak připojit tooa systémem Linux Hadoop clusteru pomocí protokolu SSH a pak použijte hello Pig příkaz toorun Pig Latin příkazy interaktivně, nebo prostřednictvím dávky úlohy."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 1da303e239b537e6b331b1d33010058582718c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-hello-pig-command-ssh"></a><span data-ttu-id="7f7f6-103">Spuštění úlohy Pig na cluster se systémem Linux s hello příkaz Pig (SSH)</span><span class="sxs-lookup"><span data-stu-id="7f7f6-103">Run Pig jobs on a Linux-based cluster with hello Pig command (SSH)</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="7f7f6-104">Zjistěte, jak toointeractively spouštět úlohy Pig z clusteru služby HDInsight tooyour připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-104">Learn how toointeractively run Pig jobs from an SSH connection tooyour HDInsight cluster.</span></span> <span data-ttu-id="7f7f6-105">Hello programovacího jazyka Pig Latin můžete toodescribe transformace, které jsou použité toohello vstupní data tooproduce hello potřeby výstup.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-105">hello Pig Latin programming language allows you toodescribe transformations that are applied toohello input data tooproduce hello desired output.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7f7f6-106">Hello kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-106">hello steps in this document require a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="7f7f6-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7f7f6-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7f7f6-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="7f7f6-109"><a id="ssh"></a>Připojení pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="7f7f6-109"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="7f7f6-110">Použití clusteru HDInsight tooyour tooconnect SSH.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-110">Use SSH tooconnect tooyour HDInsight cluster.</span></span> <span data-ttu-id="7f7f6-111">Hello následující příklad se připojí tooa clusteru s názvem **myhdinsight** jako hello účet s názvem **sshuser**:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-111">hello following example connects tooa cluster named **myhdinsight** as hello account named **sshuser**:</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

<span data-ttu-id="7f7f6-112">**Pokud jste zadali klíč certifikátu pro ověřování SSH** při vytvoření clusteru HDInsight hello budete potřebovat toospecify hello umístění hello privátní klíč klientského systému.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-112">**If you provided a certificate key for SSH authentication** when you created hello HDInsight cluster, you may need toospecify hello location of hello private key on your client system.</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

<span data-ttu-id="7f7f6-113">**Pokud jste zadali heslo pro ověřování SSH** při vytváření clusteru HDInsight hello, zadejte heslo hello po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-113">**If you provided a password for SSH authentication** when you created hello HDInsight cluster, provide hello password when prompted.</span></span>

<span data-ttu-id="7f7f6-114">Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7f7f6-114">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="7f7f6-115"><a id="pig"></a>Použijte příkaz Pig hello</span><span class="sxs-lookup"><span data-stu-id="7f7f6-115"><a id="pig"></a>Use hello Pig command</span></span>

1. <span data-ttu-id="7f7f6-116">Po připojení, spusťte hello Pig rozhraní příkazového řádku (CLI) pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-116">Once connected, start hello Pig command-line interface (CLI) by using hello following command:</span></span>

        pig

    <span data-ttu-id="7f7f6-117">Po chvíli, měli byste vidět `grunt>` řádku.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-117">After a moment, you should see a `grunt>` prompt.</span></span>

2. <span data-ttu-id="7f7f6-118">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-118">Enter hello following statement:</span></span>

        LOGS = LOAD '/example/data/sample.log';

    <span data-ttu-id="7f7f6-119">Tento příkaz načte hello obsah souboru sample.log hello do PROTOKOLŮ.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-119">This command loads hello contents of hello sample.log file into LOGS.</span></span> <span data-ttu-id="7f7f6-120">Hello obsah souboru hello můžete zobrazit pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-120">You can view hello contents of hello file by using hello following statement:</span></span>

        DUMP LOGS;

3. <span data-ttu-id="7f7f6-121">V dalším kroku transformovat hello data použitím úroveň protokolování hello pouze tooextract regulární výraz z jednotlivých záznamů pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-121">Next, transform hello data by applying a regular expression tooextract only hello logging level from each record by using hello following statement:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="7f7f6-122">Můžete použít **DUMP** tooview hello dat po transformaci hello.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-122">You can use **DUMP** tooview hello data after hello transformation.</span></span> <span data-ttu-id="7f7f6-123">V takovém případě použijte `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-123">In this case, use `DUMP LEVELS;`.</span></span>

4. <span data-ttu-id="7f7f6-124">Pokračujte v použití transformací pomocí hello příkazy v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-124">Continue applying transformations by using hello statements in hello following table:</span></span>

    | <span data-ttu-id="7f7f6-125">Pig Latin – příkaz</span><span class="sxs-lookup"><span data-stu-id="7f7f6-125">Pig Latin statement</span></span> | <span data-ttu-id="7f7f6-126">Jaké příkaz hello nemá</span><span class="sxs-lookup"><span data-stu-id="7f7f6-126">What hello statement does</span></span> |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | <span data-ttu-id="7f7f6-127">Odebere řádky, které obsahují hodnotu null pro úroveň protokolu hello a ukládá výsledky hello do `FILTEREDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-127">Removes rows that contain a null value for hello log level and stores hello results into `FILTEREDLEVELS`.</span></span> |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | <span data-ttu-id="7f7f6-128">Hello skupiny řádků podle úrovně protokolu a ukládá výsledky hello do `GROUPEDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-128">Groups hello rows by log level and stores hello results into `GROUPEDLEVELS`.</span></span> |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | <span data-ttu-id="7f7f6-129">Vytvoří sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnota úrovně a jak často se.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-129">Creates a set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="7f7f6-130">Hello datové sady je uložena do `FREQUENCIES`.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-130">hello data set is stored into `FREQUENCIES`.</span></span> |
    | `RESULT = order FREQUENCIES by COUNT desc;` | <span data-ttu-id="7f7f6-131">Řadí hello protokolu úrovně podle počtu (sestupně) a ukládá do `RESULT`.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-131">Orders hello log levels by count (descending) and stores into `RESULT`.</span></span> |

    > [!TIP]
    > <span data-ttu-id="7f7f6-132">Použití `DUMP` tooview hello výsledek hello transformace po dokončení každého kroku.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-132">Use `DUMP` tooview hello result of hello transformation after each step.</span></span>

5. <span data-ttu-id="7f7f6-133">Můžete také uložit hello výsledky transformace pomocí hello `STORE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-133">You can also save hello results of a transformation by using hello `STORE` statement.</span></span> <span data-ttu-id="7f7f6-134">Například následující příkaz hello uloží hello `RESULT` toohello `/example/data/pigout` v hello výchozí úložiště pro cluster:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-134">For example, hello following statement saves hello `RESULT` toohello `/example/data/pigout` directory on hello default storage for your cluster:</span></span>

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > <span data-ttu-id="7f7f6-135">Hello data jsou uložena v hello zadaný adresář v souborech s názvem `part-nnnnn`.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-135">hello data is stored in hello specified directory in files named `part-nnnnn`.</span></span> <span data-ttu-id="7f7f6-136">Pokud hello adresář již existuje, obdržíte chybu.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-136">If hello directory already exists, you receive an error.</span></span>

6. <span data-ttu-id="7f7f6-137">tooexit hello grunt řádku, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-137">tooexit hello grunt prompt, enter hello following statement:</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="7f7f6-138">Pig Latin dávkové soubory</span><span class="sxs-lookup"><span data-stu-id="7f7f6-138">Pig Latin batch files</span></span>

<span data-ttu-id="7f7f6-139">Můžete také použít hello Pig příkaz toorun Pig Latin obsaženého v načítaném souboru.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-139">You can also use hello Pig command toorun Pig Latin contained in a file.</span></span>

1. <span data-ttu-id="7f7f6-140">Po ukončení řádku hello grunt, použijte následující hello příkaz toopipe STDIN do souboru s názvem `pigbatch.pig`.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-140">After exiting hello grunt prompt, use hello following command toopipe STDIN into a file named `pigbatch.pig`.</span></span> <span data-ttu-id="7f7f6-141">Tento soubor je vytvořen v hello domovský adresář pro hello SSH uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-141">This file is created in hello home directory for hello SSH user account.</span></span>

        cat > ~/pigbatch.pig

2. <span data-ttu-id="7f7f6-142">Zadejte nebo vložte hello následující řádky a potom pomocí kombinace kláves Ctrl + D po dokončení.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-142">Type or paste hello following lines, and then use Ctrl+D when finished.</span></span>

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. <span data-ttu-id="7f7f6-143">Použití hello následující příkaz toorun hello `pigbatch.pig` souboru pomocí příkazu Pig hello.</span><span class="sxs-lookup"><span data-stu-id="7f7f6-143">Use hello following command toorun hello `pigbatch.pig` file by using hello Pig command.</span></span>

        pig ~/pigbatch.pig

    <span data-ttu-id="7f7f6-144">Po dokončení úlohy batch hello zobrazí hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-144">Once hello batch job finishes, you see hello following output:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <span data-ttu-id="7f7f6-145"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f7f6-145"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="7f7f6-146">Obecné informace o Pig v HDInsight najdete v části hello následujícím dokumentu:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-146">For general information on Pig in HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="7f7f6-147">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7f7f6-147">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="7f7f6-148">Další informace o dalších způsobů toowork s Hadoop v HDInsight naleznete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="7f7f6-148">For more information on other ways toowork with Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="7f7f6-149">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7f7f6-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="7f7f6-150">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7f7f6-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
