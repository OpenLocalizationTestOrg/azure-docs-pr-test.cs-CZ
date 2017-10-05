---
title: "Použijte Hadoop Pig pomocí vzdálené plochy v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití příkazu Pig spustit příkazy Pig Latin z připojení vzdálené plochy na clusteru systému Windows Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 5e8d4fbd8afc54c8bbc1a9a71c66d7022a7d5986
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a><span data-ttu-id="f4b67-103">Spuštění úlohy Pig z připojení vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="f4b67-103">Run Pig jobs from a Remote Desktop connection</span></span>
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="f4b67-104">Tento dokument poskytuje návod pro používání Pig příkaz ke spuštění příkazů Pig Latin z připojení vzdálené plochy ke clusteru HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="f4b67-104">This document provides a walkthrough for using the Pig command to run Pig Latin statements from a Remote Desktop connection to a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="f4b67-105">Pig Latin můžete vytvořit MapReduce aplikace prostřednictvím popisu transformace dat, nikoli mapování a snížit funkce.</span><span class="sxs-lookup"><span data-stu-id="f4b67-105">Pig Latin allows you to create MapReduce applications by describing data transformations, rather than map and reduce functions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f4b67-106">Vzdálená plocha je dostupná pouze na clustery HDInsight, které používají Windows jako operační systém.</span><span class="sxs-lookup"><span data-stu-id="f4b67-106">Remote Desktop is only available on HDInsight clusters that use Windows as the operating system.</span></span> <span data-ttu-id="f4b67-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="f4b67-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f4b67-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f4b67-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="f4b67-109">HDInsight 3.4 nebo větší, projděte si téma [použijte Pig s HDInsight a SSH](hdinsight-hadoop-use-pig-ssh.md) informace o interaktivním spouštění úlohy Pig přímo na clusteru z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f4b67-109">For HDInsight 3.4 or greater, see [Use Pig with HDInsight and SSH](hdinsight-hadoop-use-pig-ssh.md) for information on interactively running Pig jobs directly on the cluster from a command-line.</span></span>

## <span data-ttu-id="f4b67-110"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="f4b67-110"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="f4b67-111">Pokud chcete provést kroky v tomto článku, budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="f4b67-111">To complete the steps in this article, you will need the following.</span></span>

* <span data-ttu-id="f4b67-112">Cluster HDInsight se systémem Windows (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="f4b67-112">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="f4b67-113">Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7</span><span class="sxs-lookup"><span data-stu-id="f4b67-113">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="f4b67-114"><a id="connect"></a>Připojit pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="f4b67-114"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="f4b67-115">Povolení vzdálené plochy pro HDInsight cluster a pak připojit pomocí pokynů uvedených v [připojení ke clusterům HDInsight pomocí protokolu RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="f4b67-115">Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="f4b67-116"><a id="pig"></a>Použijte příkaz Pig</span><span class="sxs-lookup"><span data-stu-id="f4b67-116"><a id="pig"></a>Use the Pig command</span></span>
1. <span data-ttu-id="f4b67-117">Až budete mít připojení vzdálené plochy, spusťte **Hadoop příkazového řádku** pomocí ikony na ploše.</span><span class="sxs-lookup"><span data-stu-id="f4b67-117">After you have a Remote Desktop connection, start the **Hadoop Command Line** by using the icon on the desktop.</span></span>
2. <span data-ttu-id="f4b67-118">Ke spuštění příkazu Pig použijte následující:</span><span class="sxs-lookup"><span data-stu-id="f4b67-118">Use the following to start the Pig command:</span></span>

        %pig_home%\bin\pig

    <span data-ttu-id="f4b67-119">Zobrazí se `grunt>` řádku.</span><span class="sxs-lookup"><span data-stu-id="f4b67-119">You will be presented with a `grunt>` prompt.</span></span>
3. <span data-ttu-id="f4b67-120">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4b67-120">Enter the following statement:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';

    <span data-ttu-id="f4b67-121">Tento příkaz načte obsah souboru sample.log do souborů PROTOKOLŮ.</span><span class="sxs-lookup"><span data-stu-id="f4b67-121">This command loads the contents of the sample.log file into the LOGS file.</span></span> <span data-ttu-id="f4b67-122">Pomocí následujícího příkazu můžete zobrazit obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="f4b67-122">You can view the contents of the file by using the following command:</span></span>

        DUMP LOGS;
4. <span data-ttu-id="f4b67-123">Transformace dat s použitím regulárních výrazů k extrakci pouze úroveň protokolování z každý záznam:</span><span class="sxs-lookup"><span data-stu-id="f4b67-123">Transform the data by applying a regular expression to extract only the logging level from each record:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="f4b67-124">Můžete použít **DUMP** chcete zobrazit data po transformaci.</span><span class="sxs-lookup"><span data-stu-id="f4b67-124">You can use **DUMP** to view the data after the transformation.</span></span> <span data-ttu-id="f4b67-125">V takovém případě `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="f4b67-125">In this case, `DUMP LEVELS;`.</span></span>
5. <span data-ttu-id="f4b67-126">Pokračujte v použití transformací pomocí následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="f4b67-126">Continue applying transformations by using the following statements.</span></span> <span data-ttu-id="f4b67-127">Použití `DUMP` zobrazíte výsledek transformace po dokončení každého kroku.</span><span class="sxs-lookup"><span data-stu-id="f4b67-127">Use `DUMP` to view the result of the transformation after each step.</span></span>

    <table>
    <tr>
    <th><span data-ttu-id="f4b67-128">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f4b67-128">Statement</span></span></th><th><span data-ttu-id="f4b67-129">Výsledek</span><span class="sxs-lookup"><span data-stu-id="f4b67-129">What it does</span></span></th>
    </tr>
    <tr>
    <td><span data-ttu-id="f4b67-130">FILTEREDLEVELS = filtr úrovně podle LOGLEVEL není null.</span><span class="sxs-lookup"><span data-stu-id="f4b67-130">FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</span></span></td><td><span data-ttu-id="f4b67-131">Odebere řádky, které obsahují hodnotu null pro úroveň protokolu a ukládá výsledky do FILTEREDLEVELS.</span><span class="sxs-lookup"><span data-stu-id="f4b67-131">Removes rows that contain a null value for the log level and stores the results into FILTEREDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="f4b67-132">GROUPEDLEVELS = FILTEREDLEVELS skupiny podle LOGLEVEL;</span><span class="sxs-lookup"><span data-stu-id="f4b67-132">GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</span></span></td><td><span data-ttu-id="f4b67-133">Seskupí řádky podle úrovně protokolu a ukládá výsledky do GROUPEDLEVELS.</span><span class="sxs-lookup"><span data-stu-id="f4b67-133">Groups the rows by log level and stores the results into GROUPEDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="f4b67-134">FREKVENCE = foreach GROUPEDLEVELS generovat skupiny jako LOGLEVEL, počet (FILTEREDLEVELS. LOGLEVEL) jako počet;</span><span class="sxs-lookup"><span data-stu-id="f4b67-134">FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</span></span></td><td><span data-ttu-id="f4b67-135">Vytvoří novou sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnota úrovně a jak často se.</span><span class="sxs-lookup"><span data-stu-id="f4b67-135">Creates a new set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="f4b67-136">To je uložena do frekvence</span><span class="sxs-lookup"><span data-stu-id="f4b67-136">This is stored into FREQUENCIES</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="f4b67-137">VÝSLEDEK = pořadí FREKVENCÍ podle počtu desc;</span><span class="sxs-lookup"><span data-stu-id="f4b67-137">RESULT = order FREQUENCIES by COUNT desc;</span></span></td><td><span data-ttu-id="f4b67-138">Řadí úrovní záznamu do protokolu podle počtu (sestupně) a ukládá do výsledku</span><span class="sxs-lookup"><span data-stu-id="f4b67-138">Orders the log levels by count (descending,) and stores into RESULT</span></span></td>
    </tr>
    </table><span data-ttu-id="f4b67-139">
6.Můžete také uložit výsledky transformace pomocí `STORE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f4b67-139">
6. You can also save the results of a transformation by using the `STORE` statement.</span></span> <span data-ttu-id="f4b67-140">Například následující příkaz uloží `RESULT` k **/example/data/pigout** adresář ve výchozím kontejneru úložiště pro cluster:</span><span class="sxs-lookup"><span data-stu-id="f4b67-140">For example, the following command saves the `RESULT` to the **/example/data/pigout** directory in the default storage container for your cluster:</span></span>

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > <span data-ttu-id="f4b67-141">Jsou data uložena v adresáři zadané v souborech s názvem **část nnnnn**.</span><span class="sxs-lookup"><span data-stu-id="f4b67-141">The data is stored in the specified directory in files named **part-nnnnn**.</span></span> <span data-ttu-id="f4b67-142">Pokud adresář již existuje, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="f4b67-142">If the directory already exists, you will receive an error message.</span></span>
   >
   >
7. <span data-ttu-id="f4b67-143">Chcete-li ukončit řádku grunt, zadejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="f4b67-143">To exit the grunt prompt, enter the following statement.</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="f4b67-144">Pig Latin dávkové soubory</span><span class="sxs-lookup"><span data-stu-id="f4b67-144">Pig Latin batch files</span></span>
<span data-ttu-id="f4b67-145">Můžete také použít příkaz Pig ke spuštění Pig Latin, který je obsažený v souboru.</span><span class="sxs-lookup"><span data-stu-id="f4b67-145">You can also use the Pig command to run Pig Latin that is contained in a file.</span></span>

1. <span data-ttu-id="f4b67-146">Po ukončení řádku grunt, otevřete **Poznámkový blok** a vytvořte nový soubor s názvem **pigbatch.pig** v **PIG_HOME %** adresáře.</span><span class="sxs-lookup"><span data-stu-id="f4b67-146">After exiting the grunt prompt, open **Notepad** and create a new file named **pigbatch.pig** in the **%PIG_HOME%** directory.</span></span>
2. <span data-ttu-id="f4b67-147">Zadejte nebo vložte následující řádky do **pigbatch.pig** souboru a pak ho uložte:</span><span class="sxs-lookup"><span data-stu-id="f4b67-147">Type or paste the following lines into the **pigbatch.pig** file, and then save it:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. <span data-ttu-id="f4b67-148">Použijte tento příkaz ke spuštění **pigbatch.pig** souboru pomocí příkazu pig.</span><span class="sxs-lookup"><span data-stu-id="f4b67-148">Use the following to run the **pigbatch.pig** file using the pig command.</span></span>

        pig %PIG_HOME%\pigbatch.pig

    <span data-ttu-id="f4b67-149">Po dokončení úlohy batch, byste měli vidět následující výstup, který by měl být stejný jako při použití `DUMP RESULT;` v předchozích krocích:</span><span class="sxs-lookup"><span data-stu-id="f4b67-149">When the batch job completes, you should see the following output, which should be the same as when you used `DUMP RESULT;` in the previous steps:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="f4b67-150"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="f4b67-150"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="f4b67-151">Jak vidíte, příkaz Pig umožňuje interaktivně spustit MapReduce operations, nebo spustit úlohy Pig Latin, které jsou uložené v dávkovém souboru.</span><span class="sxs-lookup"><span data-stu-id="f4b67-151">As you can see, the Pig command allows you to interactively run MapReduce operations, or run Pig Latin jobs that are stored in a batch file.</span></span>

## <span data-ttu-id="f4b67-152"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4b67-152"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="f4b67-153">Obecné informace o Pig v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f4b67-153">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="f4b67-154">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f4b67-154">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="f4b67-155">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f4b67-155">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="f4b67-156">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f4b67-156">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="f4b67-157">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f4b67-157">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
