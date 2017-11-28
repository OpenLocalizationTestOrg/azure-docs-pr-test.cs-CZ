---
title: "aaaUse MapReduce a prostředí PowerShell s Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí PowerShell tooremotely spouštění úloh MapReduce s Hadoop v HDInsight."
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
ms.openlocfilehash: 59524f0e8813d4c017f92bccb2e50d4c018acf71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a><span data-ttu-id="924de-103">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="924de-103">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="924de-104">Tento dokument obsahuje příklad použití Azure PowerShell toorun úlohu MapReduce v Hadoop na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="924de-104">This document provides an example of using Azure PowerShell toorun a MapReduce job in a Hadoop on HDInsight cluster.</span></span>

## <span data-ttu-id="924de-105"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="924de-105"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="924de-106">**Cluster Azure HDInsight (Hadoop v HDInsight)**</span><span class="sxs-lookup"><span data-stu-id="924de-106">**An Azure HDInsight (Hadoop on HDInsight) cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="924de-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="924de-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="924de-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="924de-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="924de-109">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="924de-109">**A workstation with Azure PowerShell**.</span></span>

## <span data-ttu-id="924de-110"><a id="powershell"></a>Spustit úlohu MapReduce pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="924de-110"><a id="powershell"></a>Run a MapReduce job using Azure PowerShell</span></span>

<span data-ttu-id="924de-111">Prostředí Azure PowerShell poskytuje *rutiny* , umožňují úloh MapReduce tooremotely spustit v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="924de-111">Azure PowerShell provides *cmdlets* that allow you tooremotely run MapReduce jobs on HDInsight.</span></span> <span data-ttu-id="924de-112">Interně toho dosahuje pomocí volání REST příliš[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (dříve se označovaly jako Templeton) systémem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="924de-112">Internally, this is accomplished by using REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (formerly called Templeton) running on hello HDInsight cluster.</span></span>

<span data-ttu-id="924de-113">Hello se používají následující rutiny při spuštění úloh MapReduce v vzdáleného clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="924de-113">hello following cmdlets are used when running MapReduce jobs in a remote HDInsight cluster.</span></span>

* <span data-ttu-id="924de-114">**Login-AzureRmAccount**: ověřuje Azure PowerShell tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="924de-114">**Login-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure subscription.</span></span>

* <span data-ttu-id="924de-115">**Nové AzureRmHDInsightMapReduceJobDefinition**: vytvoří novou *úlohy definice* pomocí hello zadané informace o MapReduce.</span><span class="sxs-lookup"><span data-stu-id="924de-115">**New-AzureRmHDInsightMapReduceJobDefinition**: Creates a new *job definition* by using hello specified MapReduce information.</span></span>

* <span data-ttu-id="924de-116">**Spuštění AzureRmHDInsightJob**: odešle tooHDInsight definice úlohy hello, spustí hello úlohy a vrátí *úlohy* objekt, který lze použít toocheck hello stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="924de-116">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job.</span></span>

* <span data-ttu-id="924de-117">**Počkejte AzureRmHDInsightJob**: používá objekt úlohy hello toocheck hello stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="924de-117">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="924de-118">Se čeká na dokončení úlohy hello nebo je překročena doba čekání hello.</span><span class="sxs-lookup"><span data-stu-id="924de-118">It waits until hello job completes or hello wait time is exceeded.</span></span>

* <span data-ttu-id="924de-119">**Get-AzureRmHDInsightJobOutput**: používá tooretrieve hello výstup hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="924de-119">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job.</span></span>

<span data-ttu-id="924de-120">Hello následující kroky ukazují, jak toouse tyto rutiny toorun úlohu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="924de-120">hello following steps demonstrate how toouse these cmdlets toorun a job in your HDInsight cluster.</span></span>

1. <span data-ttu-id="924de-121">Pomocí editoru, uložte hello následující kód jako **mapreducejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="924de-121">Using an editor, save hello following code as **mapreducejob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. <span data-ttu-id="924de-122">Otevřete nový **prostředí Azure PowerShell** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="924de-122">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="924de-123">Umístění adresáře toohello hello změnit **mapreducejob.ps1** souboru, potom použijte následující příkaz toorun hello skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="924de-123">Change directories toohello location of hello **mapreducejob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\mapreducejob.ps1

    <span data-ttu-id="924de-124">Při spuštění skriptu hello, zobrazí se výzva k hello název clusteru HDInsight hello a hello HTTPS nebo správce účtu jména a hesla pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="924de-124">When you run hello script, you are prompted for hello name of hello HDInsight cluster and hello HTTPS/Admin account name and password for hello cluster.</span></span> <span data-ttu-id="924de-125">Může být také výzvami tooauthenticate tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="924de-125">You may also be prompted tooauthenticate tooyour Azure subscription.</span></span>

3. <span data-ttu-id="924de-126">Po dokončení úlohy hello, zobrazí se výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="924de-126">When hello job completes, you receive output similar toohello following text:</span></span>

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    <span data-ttu-id="924de-127">Tento výstup označuje, že hello úloha úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="924de-127">This output indicates that hello job completed successfully.</span></span>

    > [!NOTE]
    > <span data-ttu-id="924de-128">Pokud hello **ExitCode** je hodnota než 0, najdete v části [Poradce při potížích s](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="924de-128">If hello **ExitCode** is a value other than 0, see [Troubleshooting](#troubleshooting).</span></span>

    <span data-ttu-id="924de-129">Tento příklad také ukládá hello stažené soubory tooan **výstup.txt** soubor v adresáři hello, který spustí skript hello z.</span><span class="sxs-lookup"><span data-stu-id="924de-129">This example also stores hello downloaded files tooan **output.txt** file in hello directory that you run hello script from.</span></span>

### <a name="view-output"></a><span data-ttu-id="924de-130">Zobrazení výstupu</span><span class="sxs-lookup"><span data-stu-id="924de-130">View output</span></span>

<span data-ttu-id="924de-131">Otevřete hello **výstup.txt** soubor ve textového editoru toosee hello slova a počty vytvořené úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="924de-131">Open hello **output.txt** file in a text editor toosee hello words and counts produced by hello job.</span></span>

> [!NOTE]
> <span data-ttu-id="924de-132">výstupní soubory Hello úlohy MapReduce jsou neměnné.</span><span class="sxs-lookup"><span data-stu-id="924de-132">hello output files of a MapReduce job are immutable.</span></span> <span data-ttu-id="924de-133">Pokud tuto ukázku, takže musíte toochange hello názvu výstupního souboru hello.</span><span class="sxs-lookup"><span data-stu-id="924de-133">So if you rerun this sample, you need toochange hello name of hello output file.</span></span>

## <span data-ttu-id="924de-134"><a id="troubleshooting"></a>Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="924de-134"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="924de-135">Když se po dokončení úlohy hello nevrátí žádné informace, může mít došlo k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="924de-135">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="924de-136">informace o chybě tooview pro tuto úlohu, přidejte následující příkaz toohello konec hello hello **mapreducejob.ps1** souboru, uložit jej a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="924de-136">tooview error information for this job, add hello following command toohello end of hello **mapreducejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print hello output of hello WordCount job.
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="924de-137">Tato rutina vrátí informace hello, která byla zapsána tooSTDERR na serveru hello při spuštění úlohy hello a může pomoci zjistit, proč se nedaří hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="924de-137">This cmdlet returns hello information that was written tooSTDERR on hello server when you ran hello job, and it may help determine why hello job is failing.</span></span>

## <span data-ttu-id="924de-138"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="924de-138"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="924de-139">Jak vidíte, bude Azure PowerShell na clusteru HDInsight, stav úlohy hello monitorování a výstup hello načtení poskytuje snadný způsob toorun úloh MapReduce.</span><span class="sxs-lookup"><span data-stu-id="924de-139">As you can see, Azure PowerShell provides an easy way toorun MapReduce jobs on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="924de-140"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="924de-140"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="924de-141">Obecné informace o úloh MapReduce v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="924de-141">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="924de-142">Používání nástroje MapReduce systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="924de-142">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="924de-143">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="924de-143">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="924de-144">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="924de-144">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="924de-145">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="924de-145">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
