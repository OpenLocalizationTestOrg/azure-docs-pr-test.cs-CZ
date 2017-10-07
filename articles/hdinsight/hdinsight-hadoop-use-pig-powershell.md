---
title: "aaaUse Hadoop Pig v prostředí PowerShell v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak clusteru toosubmit Pig úlohy tooa Hadoop v HDInsight pomocí prostředí Azure PowerShell."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 771617df203011eaec715a0dba6f5014a42877f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toorun-pig-jobs-with-hdinsight"></a><span data-ttu-id="f6bf2-103">Použijte úlohy Azure PowerShell toorun Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6bf2-103">Use Azure PowerShell toorun Pig jobs with HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="f6bf2-104">Tento dokument obsahuje příklad použití Azure PowerShell toosubmit Pig úlohy tooa Hadoop na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-104">This document provides an example of using Azure PowerShell toosubmit Pig jobs tooa Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="f6bf2-105">Pig vám umožní úloh MapReduce toowrite pomocí jazyka (Pig Latin), který modelů transformace dat, nikoli mapování a snížit funkce.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-105">Pig allows you toowrite MapReduce jobs by using a language (Pig Latin) that models data transformations, rather than map and reduce functions.</span></span>

> [!NOTE]
> <span data-ttu-id="f6bf2-106">Tento dokument neposkytuje podrobný popis co dělat, příkazy Pig Latin hello použitých v ukázkách hello.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-106">This document does not provide a detailed description of what hello Pig Latin statements used in hello examples do.</span></span> <span data-ttu-id="f6bf2-107">Informace o hello Pig Latin použité v tomto příkladu najdete v tématu [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="f6bf2-107">For information about hello Pig Latin used in this example, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

## <span data-ttu-id="f6bf2-108"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="f6bf2-108"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="f6bf2-109">**Cluster Azure HDInsight**</span><span class="sxs-lookup"><span data-stu-id="f6bf2-109">**An Azure HDInsight cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="f6bf2-110">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f6bf2-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f6bf2-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="f6bf2-112">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-112">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <span data-ttu-id="f6bf2-113"><a id="powershell"></a>Spuštění úlohy Pig pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6bf2-113"><a id="powershell"></a>Run Pig jobs using PowerShell</span></span>

<span data-ttu-id="f6bf2-114">Prostředí Azure PowerShell poskytuje *rutiny* , umožňují úlohy Pig tooremotely spustit v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-114">Azure PowerShell provides *cmdlets* that allow you tooremotely run Pig jobs on HDInsight.</span></span> <span data-ttu-id="f6bf2-115">Interně, prostředí PowerShell používá volání REST příliš[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) běžící v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-115">Internally, PowerShell uses REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) running on hello HDInsight cluster.</span></span>

<span data-ttu-id="f6bf2-116">Hello se používají následující rutiny při spuštění úlohy Pig na vzdálený cluster HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f6bf2-116">hello following cmdlets are used when running Pig jobs on a remote HDInsight cluster:</span></span>

* <span data-ttu-id="f6bf2-117">**Login-AzureRmAccount**: ověřuje Azure PowerShell tooyour předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="f6bf2-117">**Login-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure Subscription</span></span>
* <span data-ttu-id="f6bf2-118">**Nové AzureRmHDInsightPigJobDefinition**: vytvoří *úlohy definice* pomocí hello zadaný Pig Latin příkazy</span><span class="sxs-lookup"><span data-stu-id="f6bf2-118">**New-AzureRmHDInsightPigJobDefinition**: Creates a *job definition* by using hello specified Pig Latin statements</span></span>
* <span data-ttu-id="f6bf2-119">**Spuštění AzureRmHDInsightJob**: odešle tooHDInsight definice úlohy hello, spustí hello úlohy a vrátí *úlohy* objekt, který lze použít toocheck hello stav úlohy hello</span><span class="sxs-lookup"><span data-stu-id="f6bf2-119">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job</span></span>
* <span data-ttu-id="f6bf2-120">**Počkejte AzureRmHDInsightJob**: používá objekt úlohy hello toocheck hello stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-120">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="f6bf2-121">Se čeká na dokončení úlohy hello nebo byla překročena doba čekání hello.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-121">It waits until hello job has completed, or hello wait time has been exceeded.</span></span>
* <span data-ttu-id="f6bf2-122">**Get-AzureRmHDInsightJobOutput**: používá tooretrieve hello výstup hello úlohy</span><span class="sxs-lookup"><span data-stu-id="f6bf2-122">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job</span></span>

<span data-ttu-id="f6bf2-123">Hello následující kroky ukazují, jak toouse tyto rutiny toorun úlohu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-123">hello following steps demonstrate how toouse these cmdlets toorun a job on your HDInsight cluster.</span></span>

1. <span data-ttu-id="f6bf2-124">Pomocí editoru, uložte hello následující kód jako **pigjob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-124">Using an editor, save hello following code as **pigjob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. <span data-ttu-id="f6bf2-125">Otevřete nový příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-125">Open a new Windows PowerShell command prompt.</span></span> <span data-ttu-id="f6bf2-126">Umístění adresáře toohello hello změnit **pigjob.ps1** souboru, potom použijte následující příkaz toorun hello skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="f6bf2-126">Change directories toohello location of hello **pigjob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\pigjob.ps1

    <span data-ttu-id="f6bf2-127">Jste výzvami toolog v tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-127">You are prompted toolog in tooyour Azure subscription.</span></span> <span data-ttu-id="f6bf2-128">Potom budete vyzváni k hello HTTPs nebo správce účtu jména a hesla pro hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-128">Then, you are asked for hello HTTPs/Admin account name and password for hello HDInsight cluster.</span></span>

2. <span data-ttu-id="f6bf2-129">Po dokončení úlohy hello, měla by vrátit informace podobná toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="f6bf2-129">When hello job completes, it should return information similar toohello following text:</span></span>

        Start hello Pig job ...
        Wait for hello Pig job toocomplete ...
        Display hello standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="f6bf2-130"><a id="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f6bf2-130"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="f6bf2-131">Když se po dokončení úlohy hello nevrátí žádné informace, může mít došlo k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-131">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="f6bf2-132">informace o chybě tooview pro tuto úlohu, přidejte následující příkaz toohello konec hello hello **pigjob.ps1** souboru, uložit jej a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-132">tooview error information for this job, add hello following command toohello end of hello **pigjob.ps1** file, save it, and then run it again.</span></span>

    # Print hello output of hello Pig job.
    Write-Host "Display hello standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

<span data-ttu-id="f6bf2-133">Vrátí hello informace, která byla zapsána tooSTDERR na serveru hello při spuštění úlohy hello a zkuste zjistit, proč se nedaří hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-133">This returns hello information that was written tooSTDERR on hello server when you ran hello job, and it may help determine why hello job is failing.</span></span>

## <span data-ttu-id="f6bf2-134"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="f6bf2-134"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="f6bf2-135">Jak můžete vidět, prostředí Azure PowerShell poskytuje snadný způsob toorun úlohy Pig v clusteru HDInsight, stav úlohy hello monitorování a načíst výstup hello.</span><span class="sxs-lookup"><span data-stu-id="f6bf2-135">As you can see, Azure PowerShell provides an easy way toorun Pig jobs on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="f6bf2-136"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6bf2-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="f6bf2-137">Obecné informace o Pig v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f6bf2-137">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="f6bf2-138">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6bf2-138">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="f6bf2-139">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f6bf2-139">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="f6bf2-140">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6bf2-140">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="f6bf2-141">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6bf2-141">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
