---
title: "aaaUse Hadoop Pig pomocí vzdálené plochy v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Pig příkaz toorun Pig Latin příkazy z clusteru systému Windows Hadoop tooa připojení vzdálené plochy v HDInsight."
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
ms.openlocfilehash: 2a4565fa827cd45fdbe6194b0486df93a6561084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a><span data-ttu-id="bf408-103">Spuštění úlohy Pig z připojení vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="bf408-103">Run Pig jobs from a Remote Desktop connection</span></span>
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="bf408-104">Tento dokument poskytuje návod pro použití hello Pig příkaz toorun Pig Latin příkazy z clusteru HDInsight se systémem Windows tooa připojení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="bf408-104">This document provides a walkthrough for using hello Pig command toorun Pig Latin statements from a Remote Desktop connection tooa Windows-based HDInsight cluster.</span></span> <span data-ttu-id="bf408-105">Pig Latin vám umožní toocreate MapReduce aplikace prostřednictvím popisu transformace dat, nikoli mapování a snížit funkce.</span><span class="sxs-lookup"><span data-stu-id="bf408-105">Pig Latin allows you toocreate MapReduce applications by describing data transformations, rather than map and reduce functions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf408-106">Vzdálená plocha je dostupná pouze na clustery HDInsight, které používají Windows jako hello operační systém.</span><span class="sxs-lookup"><span data-stu-id="bf408-106">Remote Desktop is only available on HDInsight clusters that use Windows as hello operating system.</span></span> <span data-ttu-id="bf408-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bf408-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bf408-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bf408-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="bf408-109">HDInsight 3.4 nebo větší, projděte si téma [použijte Pig s HDInsight a SSH](hdinsight-hadoop-use-pig-ssh.md) informace o interaktivním spouštění Pig úlohy přímo na hello clusteru z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="bf408-109">For HDInsight 3.4 or greater, see [Use Pig with HDInsight and SSH](hdinsight-hadoop-use-pig-ssh.md) for information on interactively running Pig jobs directly on hello cluster from a command-line.</span></span>

## <span data-ttu-id="bf408-110"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="bf408-110"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="bf408-111">toocomplete hello kroky v tomto článku, budete potřebovat následující hello.</span><span class="sxs-lookup"><span data-stu-id="bf408-111">toocomplete hello steps in this article, you will need hello following.</span></span>

* <span data-ttu-id="bf408-112">Cluster HDInsight se systémem Windows (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="bf408-112">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="bf408-113">Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7</span><span class="sxs-lookup"><span data-stu-id="bf408-113">A client computer running Windows 10, Windows 8, or Windows 7</span></span>

## <span data-ttu-id="bf408-114"><a id="connect"></a>Připojit pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="bf408-114"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="bf408-115">Povolení vzdálené plochy pro hello HDInsight cluster a pak připojit tooit podle následujících pokynů hello [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="bf408-115">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="bf408-116"><a id="pig"></a>Použijte příkaz Pig hello</span><span class="sxs-lookup"><span data-stu-id="bf408-116"><a id="pig"></a>Use hello Pig command</span></span>
1. <span data-ttu-id="bf408-117">Po připojení ke vzdálené ploše, začít hello **Hadoop příkazového řádku** pomocí hello ikony na ploše hello.</span><span class="sxs-lookup"><span data-stu-id="bf408-117">After you have a Remote Desktop connection, start hello **Hadoop Command Line** by using hello icon on hello desktop.</span></span>
2. <span data-ttu-id="bf408-118">Použijte následující příkaz Pig hello toostart hello:</span><span class="sxs-lookup"><span data-stu-id="bf408-118">Use hello following toostart hello Pig command:</span></span>

        %pig_home%\bin\pig

    <span data-ttu-id="bf408-119">Zobrazí se `grunt>` řádku.</span><span class="sxs-lookup"><span data-stu-id="bf408-119">You will be presented with a `grunt>` prompt.</span></span>
3. <span data-ttu-id="bf408-120">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bf408-120">Enter hello following statement:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';

    <span data-ttu-id="bf408-121">Tento příkaz načte obsah hello hello sample.log souboru do souboru protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="bf408-121">This command loads hello contents of hello sample.log file into hello LOGS file.</span></span> <span data-ttu-id="bf408-122">Hello obsah souboru hello můžete zobrazit pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bf408-122">You can view hello contents of hello file by using hello following command:</span></span>

        DUMP LOGS;
4. <span data-ttu-id="bf408-123">Transformace dat hello použitím úroveň protokolování hello pouze tooextract regulární výraz z jednotlivých záznamů:</span><span class="sxs-lookup"><span data-stu-id="bf408-123">Transform hello data by applying a regular expression tooextract only hello logging level from each record:</span></span>

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    <span data-ttu-id="bf408-124">Můžete použít **DUMP** tooview hello dat po transformaci hello.</span><span class="sxs-lookup"><span data-stu-id="bf408-124">You can use **DUMP** tooview hello data after hello transformation.</span></span> <span data-ttu-id="bf408-125">V takovém případě `DUMP LEVELS;`.</span><span class="sxs-lookup"><span data-stu-id="bf408-125">In this case, `DUMP LEVELS;`.</span></span>
5. <span data-ttu-id="bf408-126">Pokračujte v použití transformací pomocí hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="bf408-126">Continue applying transformations by using hello following statements.</span></span> <span data-ttu-id="bf408-127">Použití `DUMP` tooview hello výsledek hello transformace po dokončení každého kroku.</span><span class="sxs-lookup"><span data-stu-id="bf408-127">Use `DUMP` tooview hello result of hello transformation after each step.</span></span>

    <table>
    <tr>
    <th><span data-ttu-id="bf408-128">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bf408-128">Statement</span></span></th><th><span data-ttu-id="bf408-129">Výsledek</span><span class="sxs-lookup"><span data-stu-id="bf408-129">What it does</span></span></th>
    </tr>
    <tr>
    <td><span data-ttu-id="bf408-130">FILTEREDLEVELS = filtr úrovně podle LOGLEVEL není null.</span><span class="sxs-lookup"><span data-stu-id="bf408-130">FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</span></span></td><td><span data-ttu-id="bf408-131">Odebere řádky, které obsahují hodnotu null pro úroveň protokolu hello a ukládá výsledky hello do FILTEREDLEVELS.</span><span class="sxs-lookup"><span data-stu-id="bf408-131">Removes rows that contain a null value for hello log level and stores hello results into FILTEREDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="bf408-132">GROUPEDLEVELS = FILTEREDLEVELS skupiny podle LOGLEVEL;</span><span class="sxs-lookup"><span data-stu-id="bf408-132">GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</span></span></td><td><span data-ttu-id="bf408-133">Hello skupiny řádků podle úrovně protokolu a ukládá výsledky hello do GROUPEDLEVELS.</span><span class="sxs-lookup"><span data-stu-id="bf408-133">Groups hello rows by log level and stores hello results into GROUPEDLEVELS.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="bf408-134">FREKVENCE = foreach GROUPEDLEVELS generovat skupiny jako LOGLEVEL, počet (FILTEREDLEVELS. LOGLEVEL) jako počet;</span><span class="sxs-lookup"><span data-stu-id="bf408-134">FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</span></span></td><td><span data-ttu-id="bf408-135">Vytvoří novou sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnota úrovně a jak často se.</span><span class="sxs-lookup"><span data-stu-id="bf408-135">Creates a new set of data that contains each unique log level value and how many times it occurs.</span></span> <span data-ttu-id="bf408-136">To je uložena do frekvence</span><span class="sxs-lookup"><span data-stu-id="bf408-136">This is stored into FREQUENCIES</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="bf408-137">VÝSLEDEK = pořadí FREKVENCÍ podle počtu desc;</span><span class="sxs-lookup"><span data-stu-id="bf408-137">RESULT = order FREQUENCIES by COUNT desc;</span></span></td><td><span data-ttu-id="bf408-138">Řadí hello protokolu úrovně podle počtu (sestupně) a úložiště do výsledku</span><span class="sxs-lookup"><span data-stu-id="bf408-138">Orders hello log levels by count (descending,) and stores into RESULT</span></span></td>
    </tr>
    </table><span data-ttu-id="bf408-139">
6.Můžete také uložit hello výsledky transformace pomocí hello `STORE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="bf408-139">
6. You can also save hello results of a transformation by using hello `STORE` statement.</span></span> <span data-ttu-id="bf408-140">Například následující příkaz hello uloží hello `RESULT` toohello **/example/data/pigout** adresář v hello výchozí kontejner úložiště pro cluster:</span><span class="sxs-lookup"><span data-stu-id="bf408-140">For example, hello following command saves hello `RESULT` toohello **/example/data/pigout** directory in hello default storage container for your cluster:</span></span>

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > <span data-ttu-id="bf408-141">Hello data jsou uložena v hello zadaný adresář v souborech s názvem **část nnnnn**.</span><span class="sxs-lookup"><span data-stu-id="bf408-141">hello data is stored in hello specified directory in files named **part-nnnnn**.</span></span> <span data-ttu-id="bf408-142">Pokud hello adresář již existuje, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="bf408-142">If hello directory already exists, you will receive an error message.</span></span>
   >
   >
7. <span data-ttu-id="bf408-143">tooexit hello grunt řádku, zadejte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="bf408-143">tooexit hello grunt prompt, enter hello following statement.</span></span>

        QUIT;

### <a name="pig-latin-batch-files"></a><span data-ttu-id="bf408-144">Pig Latin dávkové soubory</span><span class="sxs-lookup"><span data-stu-id="bf408-144">Pig Latin batch files</span></span>
<span data-ttu-id="bf408-145">Můžete taky hello Pig příkaz toorun Pig Latin, který je obsažen v souboru.</span><span class="sxs-lookup"><span data-stu-id="bf408-145">You can also use hello Pig command toorun Pig Latin that is contained in a file.</span></span>

1. <span data-ttu-id="bf408-146">Po ukončení řádku hello grunt, otevřete **Poznámkový blok** a vytvořte nový soubor s názvem **pigbatch.pig** v hello **PIG_HOME %** adresáře.</span><span class="sxs-lookup"><span data-stu-id="bf408-146">After exiting hello grunt prompt, open **Notepad** and create a new file named **pigbatch.pig** in hello **%PIG_HOME%** directory.</span></span>
2. <span data-ttu-id="bf408-147">Typ nebo následující hello vložení řádků do hello **pigbatch.pig** souboru a pak ho uložte:</span><span class="sxs-lookup"><span data-stu-id="bf408-147">Type or paste hello following lines into hello **pigbatch.pig** file, and then save it:</span></span>

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. <span data-ttu-id="bf408-148">Použití hello následující toorun hello **pigbatch.pig** soubor pomocí příkazu pig hello.</span><span class="sxs-lookup"><span data-stu-id="bf408-148">Use hello following toorun hello **pigbatch.pig** file using hello pig command.</span></span>

        pig %PIG_HOME%\pigbatch.pig

    <span data-ttu-id="bf408-149">Po dokončení úlohy batch hello byste měli vidět následující výstupu, který by měl být hello stejný jako při použití hello `DUMP RESULT;` v předchozích krocích hello:</span><span class="sxs-lookup"><span data-stu-id="bf408-149">When hello batch job completes, you should see hello following output, which should be hello same as when you used `DUMP RESULT;` in hello previous steps:</span></span>

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="bf408-150"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="bf408-150"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="bf408-151">Jak vidíte, hello Pig příkaz vám umožní toointeractively spustit MapReduce operations, nebo spustit úlohy Pig Latin, které jsou uložené v dávkovém souboru.</span><span class="sxs-lookup"><span data-stu-id="bf408-151">As you can see, hello Pig command allows you toointeractively run MapReduce operations, or run Pig Latin jobs that are stored in a batch file.</span></span>

## <span data-ttu-id="bf408-152"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf408-152"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="bf408-153">Obecné informace o Pig v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="bf408-153">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="bf408-154">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="bf408-154">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="bf408-155">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="bf408-155">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="bf408-156">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="bf408-156">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="bf408-157">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="bf408-157">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
