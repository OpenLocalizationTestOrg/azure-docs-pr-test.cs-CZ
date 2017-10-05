---
title: "Použijte Hadoop Pig v prostředí PowerShell v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak úlohy Pig do clusteru Hadoop v HDInsight pomocí prostředí Azure PowerShell."
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
ms.openlocfilehash: 28904b07609ffb40a8195278fd1afd3957896733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a><span data-ttu-id="f0fa6-103">Pomocí prostředí Azure PowerShell ke spuštění úlohy Pig s HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0fa6-103">Use Azure PowerShell to run Pig jobs with HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="f0fa6-104">Tento dokument poskytuje příklad použití Azure PowerShell k odesílání úloh Pig do Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-104">This document provides an example of using Azure PowerShell to submit Pig jobs to a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="f0fa6-105">Pig umožňuje zapsat úloh MapReduce pomocí jazyka (Pig Latin) této transformace dat modely, nikoli mapování a snížit funkce.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-105">Pig allows you to write MapReduce jobs by using a language (Pig Latin) that models data transformations, rather than map and reduce functions.</span></span>

> [!NOTE]
> <span data-ttu-id="f0fa6-106">Tento dokument neposkytuje podrobný popis co dělat, Pig Latin příkazy použít v příkladech.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-106">This document does not provide a detailed description of what the Pig Latin statements used in the examples do.</span></span> <span data-ttu-id="f0fa6-107">Informace o Pig Latin použité v tomto příkladu najdete v tématu [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="f0fa6-107">For information about the Pig Latin used in this example, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

## <span data-ttu-id="f0fa6-108"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="f0fa6-108"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="f0fa6-109">**Cluster Azure HDInsight**</span><span class="sxs-lookup"><span data-stu-id="f0fa6-109">**An Azure HDInsight cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="f0fa6-110">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f0fa6-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f0fa6-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="f0fa6-112">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-112">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <span data-ttu-id="f0fa6-113"><a id="powershell"></a>Spuštění úlohy Pig pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0fa6-113"><a id="powershell"></a>Run Pig jobs using PowerShell</span></span>

<span data-ttu-id="f0fa6-114">Prostředí Azure PowerShell poskytuje *rutiny* které umožňují vzdáleně spouštět úlohy Pig v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-114">Azure PowerShell provides *cmdlets* that allow you to remotely run Pig jobs on HDInsight.</span></span> <span data-ttu-id="f0fa6-115">Interně, prostředí PowerShell používá volání REST [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) běžící v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-115">Internally, PowerShell uses REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) running on the HDInsight cluster.</span></span>

<span data-ttu-id="f0fa6-116">Při spuštění úlohy Pig na vzdálený cluster HDInsight, se používají následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="f0fa6-116">The following cmdlets are used when running Pig jobs on a remote HDInsight cluster:</span></span>

* <span data-ttu-id="f0fa6-117">**Login-AzureRmAccount**: ověřuje prostředí Azure PowerShell k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="f0fa6-117">**Login-AzureRmAccount**: Authenticates Azure PowerShell to your Azure Subscription</span></span>
* <span data-ttu-id="f0fa6-118">**Nové AzureRmHDInsightPigJobDefinition**: vytvoří *úlohy definice* pomocí zadané Pig Latin příkazy</span><span class="sxs-lookup"><span data-stu-id="f0fa6-118">**New-AzureRmHDInsightPigJobDefinition**: Creates a *job definition* by using the specified Pig Latin statements</span></span>
* <span data-ttu-id="f0fa6-119">**Spuštění AzureRmHDInsightJob**: odešle definici úlohy do HDInsight, spustí úlohu a vrátí *úlohy* objekt, který můžete použít ke kontrole stavu úlohy</span><span class="sxs-lookup"><span data-stu-id="f0fa6-119">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job</span></span>
* <span data-ttu-id="f0fa6-120">**Počkejte AzureRmHDInsightJob**: používá objekt úlohy a zkontrolujte stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-120">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="f0fa6-121">Se čeká na dokončení úlohy nebo byla překročena doba čekání.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-121">It waits until the job has completed, or the wait time has been exceeded.</span></span>
* <span data-ttu-id="f0fa6-122">**Get-AzureRmHDInsightJobOutput**: používá se k načtení výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="f0fa6-122">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job</span></span>

<span data-ttu-id="f0fa6-123">Následující kroky ukazují, jak tyto rutiny použít ke spuštění úlohy v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-123">The following steps demonstrate how to use these cmdlets to run a job on your HDInsight cluster.</span></span>

1. <span data-ttu-id="f0fa6-124">Pomocí editoru, uložte následující kód jako **pigjob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-124">Using an editor, save the following code as **pigjob.ps1**.</span></span>

    <span data-ttu-id="f0fa6-125">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]</span><span class="sxs-lookup"><span data-stu-id="f0fa6-125">[!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]</span></span>

1. <span data-ttu-id="f0fa6-126">Otevřete nový příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-126">Open a new Windows PowerShell command prompt.</span></span> <span data-ttu-id="f0fa6-127">Změňte adresáře na umístění **pigjob.ps1** souboru a potom použijte následující příkaz pro spuštění skriptu:</span><span class="sxs-lookup"><span data-stu-id="f0fa6-127">Change directories to the location of the **pigjob.ps1** file, then use the following command to run the script:</span></span>

        .\pigjob.ps1

    <span data-ttu-id="f0fa6-128">Zobrazí se výzva k přihlášení k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-128">You are prompted to log in to your Azure subscription.</span></span> <span data-ttu-id="f0fa6-129">Potom budete vyzváni k HTTPs nebo správce název účtu a hesla pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-129">Then, you are asked for the HTTPs/Admin account name and password for the HDInsight cluster.</span></span>

2. <span data-ttu-id="f0fa6-130">Po dokončení úlohy, měla by vrátit informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="f0fa6-130">When the job completes, it should return information similar to the following text:</span></span>

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="f0fa6-131"><a id="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f0fa6-131"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="f0fa6-132">Pokud žádné informace se vrátí po dokončení úlohy, mohlo dojít k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-132">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="f0fa6-133">Chcete-li zobrazit informace o chybě pro tuto úlohu, přidejte následující příkaz na konec **pigjob.ps1** souboru, uložit jej a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-133">To view error information for this job, add the following command to the end of the **pigjob.ps1** file, save it, and then run it again.</span></span>

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

<span data-ttu-id="f0fa6-134">Tento příkaz vrátí informace, které byla zapsána do STDERR na serveru při spuštění úlohy a ho mohou pomoci zjistit, proč je neúspěšné úlohy.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-134">This returns the information that was written to STDERR on the server when you ran the job, and it may help determine why the job is failing.</span></span>

## <span data-ttu-id="f0fa6-135"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="f0fa6-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="f0fa6-136">Jak vidíte, Azure PowerShell poskytuje snadný způsob, jak spouštět úlohy Pig v clusteru HDInsight, sledovat stav úlohy a načíst výstup.</span><span class="sxs-lookup"><span data-stu-id="f0fa6-136">As you can see, Azure PowerShell provides an easy way to run Pig jobs on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="f0fa6-137"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0fa6-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="f0fa6-138">Obecné informace o Pig v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f0fa6-138">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="f0fa6-139">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0fa6-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="f0fa6-140">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f0fa6-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="f0fa6-141">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0fa6-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="f0fa6-142">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f0fa6-142">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
