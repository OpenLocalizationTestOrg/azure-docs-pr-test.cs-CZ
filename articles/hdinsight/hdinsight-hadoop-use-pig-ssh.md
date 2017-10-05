---
title: "Použijte Hadoop Pig pomocí protokolu SSH v clusteru HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak připojit do clusteru se systémem Linux Hadoop s SSH a pak použijte příkaz Pig ke spuštění příkazů Pig Latin interaktivně, nebo jako dávkovou úlohu."
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
ms.openlocfilehash: e4c893ef4bfa573dd9fbc9c9b0ae296720769842
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a><span data-ttu-id="ab644-103">Spuštění úlohy Pig na cluster se systémem Linux pomocí příkazu Pig (SSH)</span><span class="sxs-lookup"><span data-stu-id="ab644-103">Run Pig jobs on a Linux-based cluster with the Pig command (SSH)</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="ab644-104">Zjistěte, jak interaktivně spouštět úlohy Pig ze připojení SSH ke svému clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ab644-104">Learn how to interactively run Pig jobs from an SSH connection to your HDInsight cluster.</span></span> <span data-ttu-id="ab644-105">Programovací jazyk Pig Latin můžete k popisu transformace, které se použijí u vstupních dat k vytvoření požadované výstup.</span><span class="sxs-lookup"><span data-stu-id="ab644-105">The Pig Latin programming language allows you to describe transformations that are applied to the input data to produce the desired output.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab644-106">Kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="ab644-106">The steps in this document require a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ab644-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ab644-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ab644-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ab644-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="ab644-109"><a id="ssh"></a>Připojení pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="ab644-109"><a id="ssh"></a>Connect with SSH</span></span>

<span data-ttu-id="ab644-110">Použití SSH se připojit ke svému clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ab644-110">Use SSH to connect to your HDInsight cluster.</span></span> <span data-ttu-id="ab644-111">Následující příklad se připojí ke clusteru s názvem **myhdinsight** jako účet s názvem **sshuser**:</span><span class="sxs-lookup"><span data-stu-id="ab644-111">The following example connects to a cluster named **myhdinsight** as the account named **sshuser**:</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

<span data-ttu-id="ab644-112">**Pokud jste zadali klíč certifikátu pro ověřování SSH** při vytváření clusteru HDInsight, možná muset zadat umístění privátní klíč klientského systému.</span><span class="sxs-lookup"><span data-stu-id="ab644-112">**If you provided a certificate key for SSH authentication** when you created the HDInsight cluster, you may need to specify the location of the private key on your client system.</span></span>

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

<span data-ttu-id="ab644-113">**Pokud jste zadali heslo pro ověřování SSH** při vytváření clusteru HDInsight, zadejte po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="ab644-113">**If you provided a password for SSH authentication** when you created the HDInsight cluster, provide the password when prompted.</span></span>

<span data-ttu-id="ab644-114">Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ab644-114">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="ab644-115"><a id="pig"></a>Použijte příkaz Pig</span><span class="sxs-lookup"><span data-stu-id="ab644-115"><a id="pig"></a>Use the Pig command</span></span>

1. <span data-ttu-id="ab644-116">Po připojení pomocí následujícího příkazu spusťte Pig rozhraní příkazového řádku (CLI):</span><span class="sxs-lookup"><span data-stu-id="ab644-116">Once connected, start the Pig command-line interface (CLI) by using the following command:</span></span>

        pig

    <span data-ttu-id="ab644-117">Po chvíli, měli byste vidět `grunt>` řádku.</span><span class="sxs-lookup"><span data-stu-id="ab644-117">After a moment, you should see a `grunt>` prompt.</span></span>

2. <span data-ttu-id="ab644-118">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab644-118">Enter the following statement:</span></span>

        LOGS = LOAD '/example/data/sample.log';

    <span data-ttu-id="ab644-119">Tento příkaz načte obsah souboru sample.log do PROTOKOLŮ.</span><span class="sxs-lookup"><span data-stu-id="ab644-119">This command loads the contents of the sample.log file into LOGS.</span></span> <span data-ttu-id="ab644-120">Pomocí následujícího příkazu můžete zobrazit obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="ab644-120">You can view the contents of the file by using the following statement:</span></span>

        DUMP LOGS;

3. <span data-ttu-id="ab644-121">V dalším kroku transformaci dat s použitím regulárních výrazů k extrakci pouze úroveň protokolování z každý záznam pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="ab644-121">Next, transform the data by applying a regular expression to extract only the logging level from each record by using the following statement:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="ab644-122">Můžete použít **DUMP** chcete zobrazit data po transformaci.</span><span class="sxs-lookup"><span data-stu-id="ab644-122">You can use **DUMP** to view the data after the transformation.</span></span> <span data-ttu-id="ab644-123">V takovém případě použijte `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="ab644-123">In this case, use `DUMP LEVELS;`.</span></span>

4. <span data-ttu-id="ab644-124">Pokračujte v použití transformací pomocí příkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="ab644-124">Continue applying transformations by using the statements in the following table:</span></span>

    | <span data-ttu-id="ab644-125">Pig Latin – příkaz</span><span class="sxs-lookup"><span data-stu-id="ab644-125">Pig Latin statement</span></span> | <span data-ttu-id="ab644-126">Jaké jsou příkaz</span><span class="sxs-lookup"><span data-stu-id="ab644-126">What the statement does</span></span> |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | <span data-ttu-id="ab644-127">Odebere řádky, které obsahují hodnotu null pro úroveň protokolu a ukládá výsledky do `FILTEREDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="ab644-127">Removes rows that contain a null value for the log level and stores the results into `FILTEREDLEVELS`.</span></span> |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | <span data-ttu-id="ab644-128">Skupiny řádků a úroveň protokolu a ukládá výsledky do `GROUPEDLEVELS`.</span><span class="sxs-lookup"><span data-stu-id="ab644-128">Groups the rows by log level and stores the results into `GROUPEDLEVELS`.</span></span> |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | <span data-ttu-id="ab644-129">Vytvoří sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnota úrovně a jak často se.</span><span class="sxs-lookup"><span data-stu-id="ab644-129">Creates a set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="ab644-130">Datová sada ukládána do `FREQUENCIES`.</span><span class="sxs-lookup"><span data-stu-id="ab644-130">The data set is stored into `FREQUENCIES`.</span></span> |
    | `RESULT = order FREQUENCIES by COUNT desc;` | <span data-ttu-id="ab644-131">Řadí úrovní záznamu do protokolu podle počtu (sestupně) a ukládá do `RESULT`.</span><span class="sxs-lookup"><span data-stu-id="ab644-131">Orders the log levels by count (descending) and stores into `RESULT`.</span></span> |

    > [!TIP]
    > <span data-ttu-id="ab644-132">Použití `DUMP` zobrazíte výsledek transformace po dokončení každého kroku.</span><span class="sxs-lookup"><span data-stu-id="ab644-132">Use `DUMP` to view the result of the transformation after each step.</span></span>

5. <span data-ttu-id="ab644-133">Můžete také uložit výsledky transformace pomocí `STORE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="ab644-133">You can also save the results of a transformation by using the `STORE` statement.</span></span> <span data-ttu-id="ab644-134">Například následující příkaz uloží `RESULT` k `/example/data/pigout` adresář na výchozí úložiště pro cluster:</span><span class="sxs-lookup"><span data-stu-id="ab644-134">For example, the following statement saves the `RESULT` to the `/example/data/pigout` directory on the default storage for your cluster:</span></span>

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > <span data-ttu-id="ab644-135">Jsou data uložena v adresáři zadané v souborech s názvem `part-nnnnn`.</span><span class="sxs-lookup"><span data-stu-id="ab644-135">The data is stored in the specified directory in files named `part-nnnnn`.</span></span> <span data-ttu-id="ab644-136">Pokud adresář již existuje, obdržíte chybu.</span><span class="sxs-lookup"><span data-stu-id="ab644-136">If the directory already exists, you receive an error.</span></span>

6. <span data-ttu-id="ab644-137">Chcete-li ukončit řádku grunt, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab644-137">To exit the grunt prompt, enter the following statement:</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="ab644-138">Pig Latin dávkové soubory</span><span class="sxs-lookup"><span data-stu-id="ab644-138">Pig Latin batch files</span></span>

<span data-ttu-id="ab644-139">Můžete také použít příkaz Pig ke spuštění Pig Latin obsaženého v načítaném souboru.</span><span class="sxs-lookup"><span data-stu-id="ab644-139">You can also use the Pig command to run Pig Latin contained in a file.</span></span>

1. <span data-ttu-id="ab644-140">Po ukončení řádku grunt, použijte následující příkaz k kanálu STDIN do souboru s názvem `pigbatch.pig`.</span><span class="sxs-lookup"><span data-stu-id="ab644-140">After exiting the grunt prompt, use the following command to pipe STDIN into a file named `pigbatch.pig`.</span></span> <span data-ttu-id="ab644-141">Tento soubor je vytvořen v domovském adresáři pro uživatelský účet SSH.</span><span class="sxs-lookup"><span data-stu-id="ab644-141">This file is created in the home directory for the SSH user account.</span></span>

        cat > ~/pigbatch.pig

2. <span data-ttu-id="ab644-142">Zadejte nebo vložte následující řádky a potom pomocí kombinace kláves Ctrl + D po dokončení.</span><span class="sxs-lookup"><span data-stu-id="ab644-142">Type or paste the following lines, and then use Ctrl+D when finished.</span></span>

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. <span data-ttu-id="ab644-143">Použijte následující příkaz ke spuštění `pigbatch.pig` souboru pomocí příkazu Pig.</span><span class="sxs-lookup"><span data-stu-id="ab644-143">Use the following command to run the `pigbatch.pig` file by using the Pig command.</span></span>

        pig ~/pigbatch.pig

    <span data-ttu-id="ab644-144">Po dokončení úlohy batch, zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="ab644-144">Once the batch job finishes, you see the following output:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <span data-ttu-id="ab644-145"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab644-145"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="ab644-146">Obecné informace o Pig v HDInsight najdete v následujícím dokumentu:</span><span class="sxs-lookup"><span data-stu-id="ab644-146">For general information on Pig in HDInsight, see the following document:</span></span>

* [<span data-ttu-id="ab644-147">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab644-147">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="ab644-148">Další informace o další způsoby, jak pracovat s Hadoop v HDInsight najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="ab644-148">For more information on other ways to work with Hadoop on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="ab644-149">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab644-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ab644-150">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab644-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
