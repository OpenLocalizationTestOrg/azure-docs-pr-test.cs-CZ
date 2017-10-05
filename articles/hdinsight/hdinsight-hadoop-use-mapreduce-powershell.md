---
title: "Používání MapReduce a prostředí PowerShell s Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak pomocí prostředí PowerShell vzdáleně spouštět úlohy MapReduce s Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: c3801573808709f29cb1e563ac803f225a28cafc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a><span data-ttu-id="e5633-103">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5633-103">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="e5633-104">Tento dokument poskytuje příklad použití Azure PowerShell a spusťte úlohu MapReduce v Hadoop na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e5633-104">This document provides an example of using Azure PowerShell to run a MapReduce job in a Hadoop on HDInsight cluster.</span></span>

## <span data-ttu-id="e5633-105"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="e5633-105"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="e5633-106">**Cluster Azure HDInsight (Hadoop v HDInsight)**</span><span class="sxs-lookup"><span data-stu-id="e5633-106">**An Azure HDInsight (Hadoop on HDInsight) cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e5633-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="e5633-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e5633-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e5633-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="e5633-109">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="e5633-109">**A workstation with Azure PowerShell**.</span></span>

## <span data-ttu-id="e5633-110"><a id="powershell"></a>Spustit úlohu MapReduce pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5633-110"><a id="powershell"></a>Run a MapReduce job using Azure PowerShell</span></span>

<span data-ttu-id="e5633-111">Prostředí Azure PowerShell poskytuje *rutiny* které umožňují vzdáleně spouštět úlohy MapReduce v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e5633-111">Azure PowerShell provides *cmdlets* that allow you to remotely run MapReduce jobs on HDInsight.</span></span> <span data-ttu-id="e5633-112">Interně toho dosahuje pomocí volání REST [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (dříve se označovaly jako Templeton) spuštěná na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e5633-112">Internally, this is accomplished by using REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (formerly called Templeton) running on the HDInsight cluster.</span></span>

<span data-ttu-id="e5633-113">Při spouštění úloh MapReduce v vzdáleného clusteru HDInsight, se používají následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="e5633-113">The following cmdlets are used when running MapReduce jobs in a remote HDInsight cluster.</span></span>

* <span data-ttu-id="e5633-114">**Login-AzureRmAccount**: ověřuje prostředí Azure PowerShell k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="e5633-114">**Login-AzureRmAccount**: Authenticates Azure PowerShell to your Azure subscription.</span></span>

* <span data-ttu-id="e5633-115">**Nové AzureRmHDInsightMapReduceJobDefinition**: vytvoří novou *úlohy definice* pomocí zadané informace o MapReduce.</span><span class="sxs-lookup"><span data-stu-id="e5633-115">**New-AzureRmHDInsightMapReduceJobDefinition**: Creates a new *job definition* by using the specified MapReduce information.</span></span>

* <span data-ttu-id="e5633-116">**Spuštění AzureRmHDInsightJob**: odešle definici úlohy do HDInsight, spustí úlohu a vrátí *úlohy* objekt, který můžete použít ke kontrole stavu úlohy.</span><span class="sxs-lookup"><span data-stu-id="e5633-116">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job.</span></span>

* <span data-ttu-id="e5633-117">**Počkejte AzureRmHDInsightJob**: používá objekt úlohy a zkontrolujte stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="e5633-117">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="e5633-118">Se čeká na dokončení úlohy nebo je překročena doba čekání.</span><span class="sxs-lookup"><span data-stu-id="e5633-118">It waits until the job completes or the wait time is exceeded.</span></span>

* <span data-ttu-id="e5633-119">**Get-AzureRmHDInsightJobOutput**: používá se k načtení výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="e5633-119">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job.</span></span>

<span data-ttu-id="e5633-120">Následující kroky ukazují, jak tyto rutiny použít ke spuštění úlohy v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e5633-120">The following steps demonstrate how to use these cmdlets to run a job in your HDInsight cluster.</span></span>

1. <span data-ttu-id="e5633-121">Pomocí editoru, uložte následující kód jako **mapreducejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="e5633-121">Using an editor, save the following code as **mapreducejob.ps1**.</span></span>

    <span data-ttu-id="e5633-122">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]</span><span class="sxs-lookup"><span data-stu-id="e5633-122">[!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]</span></span>

2. <span data-ttu-id="e5633-123">Otevřete nový **prostředí Azure PowerShell** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e5633-123">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="e5633-124">Změňte adresáře na umístění **mapreducejob.ps1** souboru a potom použijte následující příkaz pro spuštění skriptu:</span><span class="sxs-lookup"><span data-stu-id="e5633-124">Change directories to the location of the **mapreducejob.ps1** file, then use the following command to run the script:</span></span>

        .\mapreducejob.ps1

    <span data-ttu-id="e5633-125">Když spustíte skript, zobrazí se výzva k název clusteru HDInsight a HTTPS nebo správce název účtu a hesla pro cluster.</span><span class="sxs-lookup"><span data-stu-id="e5633-125">When you run the script, you are prompted for the name of the HDInsight cluster and the HTTPS/Admin account name and password for the cluster.</span></span> <span data-ttu-id="e5633-126">Také můžete být vyzváni k ověření vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="e5633-126">You may also be prompted to authenticate to your Azure subscription.</span></span>

3. <span data-ttu-id="e5633-127">Po dokončení úlohy, zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="e5633-127">When the job completes, you receive output similar to the following text:</span></span>

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    <span data-ttu-id="e5633-128">Tento výstup označuje, že úloha byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="e5633-128">This output indicates that the job completed successfully.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e5633-129">Pokud **ExitCode** je hodnota než 0, najdete v části [Poradce při potížích s](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="e5633-129">If the **ExitCode** is a value other than 0, see [Troubleshooting](#troubleshooting).</span></span>

    <span data-ttu-id="e5633-130">Tento příklad také ukládá stažené soubory, které chcete **výstup.txt** soubor v adresáři, který spuštění skriptu z.</span><span class="sxs-lookup"><span data-stu-id="e5633-130">This example also stores the downloaded files to an **output.txt** file in the directory that you run the script from.</span></span>

### <a name="view-output"></a><span data-ttu-id="e5633-131">Zobrazení výstupu</span><span class="sxs-lookup"><span data-stu-id="e5633-131">View output</span></span>

<span data-ttu-id="e5633-132">Otevřete **výstup.txt** soubor v textovém editoru slova a počty vytvořeného úlohou.</span><span class="sxs-lookup"><span data-stu-id="e5633-132">Open the **output.txt** file in a text editor to see the words and counts produced by the job.</span></span>

> [!NOTE]
> <span data-ttu-id="e5633-133">Výstupní soubory úlohy MapReduce jsou neměnné.</span><span class="sxs-lookup"><span data-stu-id="e5633-133">The output files of a MapReduce job are immutable.</span></span> <span data-ttu-id="e5633-134">Proto pokud tuto ukázku, musíte změnit název výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="e5633-134">So if you rerun this sample, you need to change the name of the output file.</span></span>

## <span data-ttu-id="e5633-135"><a id="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="e5633-135"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="e5633-136">Pokud žádné informace se vrátí po dokončení úlohy, mohlo dojít k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="e5633-136">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="e5633-137">Chcete-li zobrazit informace o chybě pro tuto úlohu, přidejte následující příkaz na konec **mapreducejob.ps1** souboru, uložit jej a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="e5633-137">To view error information for this job, add the following command to the end of the **mapreducejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="e5633-138">Tato rutina vrátí informace, která byla zapsána do STDERR na serveru při spuštění úlohy a může pomoci zjistit, proč se nedaří úlohy.</span><span class="sxs-lookup"><span data-stu-id="e5633-138">This cmdlet returns the information that was written to STDERR on the server when you ran the job, and it may help determine why the job is failing.</span></span>

## <span data-ttu-id="e5633-139"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="e5633-139"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="e5633-140">Jak vidíte, Azure PowerShell poskytuje snadný způsob, jak spouštět úlohy MapReduce v clusteru HDInsight, sledovat stav úlohy a načíst výstup.</span><span class="sxs-lookup"><span data-stu-id="e5633-140">As you can see, Azure PowerShell provides an easy way to run MapReduce jobs on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="e5633-141"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="e5633-141"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="e5633-142">Obecné informace o úloh MapReduce v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e5633-142">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="e5633-143">Používání nástroje MapReduce systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="e5633-143">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="e5633-144">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e5633-144">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="e5633-145">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e5633-145">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="e5633-146">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e5633-146">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
