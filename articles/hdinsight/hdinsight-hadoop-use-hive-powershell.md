---
title: "Používání Hadoop Hive v prostředí PowerShell v HDInsight - Azure | Microsoft Docs"
description: "Pomocí prostředí PowerShell ke spouštění dotazů Hive v Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: e1cb2e4a1fc82fb43082e79a5feba71b81b3eaa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-hive-queries-using-powershell"></a><span data-ttu-id="a4e90-103">Spouštění dotazů Hive pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a4e90-103">Run Hive queries using PowerShell</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="a4e90-104">Tento dokument poskytuje příklad použití Azure PowerShell v režimu skupiny prostředků Azure ke spouštění dotazů Hive v Hadoop na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a4e90-104">This document provides an example of using Azure PowerShell in the Azure Resource Group mode to run Hive queries in a Hadoop on HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="a4e90-105">Tento dokument neposkytuje podrobný popis co dělat, příkazy HiveQL, které se používají v příkladech.</span><span class="sxs-lookup"><span data-stu-id="a4e90-105">This document does not provide a detailed description of what the HiveQL statements that are used in the examples do.</span></span> <span data-ttu-id="a4e90-106">Informace o HiveQL, který se používá v tomto příkladu najdete v tématu [používání Hive s Hadoop v HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="a4e90-106">For information on the HiveQL that is used in this example, see [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="a4e90-107">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="a4e90-107">**Prerequisites**</span></span>

* <span data-ttu-id="a4e90-108">**Cluster Azure HDInsight**: nezáleží, jestli je clusteru systému Windows nebo linuxu.</span><span class="sxs-lookup"><span data-stu-id="a4e90-108">**An Azure HDInsight cluster**: It does not matter whether the cluster is Windows or Linux-based.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a4e90-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a4e90-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a4e90-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a4e90-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="a4e90-111">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="a4e90-111">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a><span data-ttu-id="a4e90-112">Spouštění dotazů Hive pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a4e90-112">Run Hive queries using Azure PowerShell</span></span>

<span data-ttu-id="a4e90-113">Prostředí Azure PowerShell poskytuje *rutiny* které umožňují vzdáleně spouštět dotazy Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a4e90-113">Azure PowerShell provides *cmdlets* that allow you to remotely run Hive queries on HDInsight.</span></span> <span data-ttu-id="a4e90-114">Interně rutiny volat REST [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a4e90-114">Internally, the cmdlets make REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on the HDInsight cluster.</span></span>

<span data-ttu-id="a4e90-115">Při spouštění dotazů Hive v vzdáleného clusteru HDInsight, se používají následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="a4e90-115">The following cmdlets are used when running Hive queries in a remote HDInsight cluster:</span></span>

* <span data-ttu-id="a4e90-116">**Přidat-AzureRmAccount**: ověřuje prostředí Azure PowerShell k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="a4e90-116">**Add-AzureRmAccount**: Authenticates Azure PowerShell to your Azure subscription</span></span>
* <span data-ttu-id="a4e90-117">**Nové AzureRmHDInsightHiveJobDefinition**: vytvoří *úlohy definice* pomocí zadané příkazy HiveQL</span><span class="sxs-lookup"><span data-stu-id="a4e90-117">**New-AzureRmHDInsightHiveJobDefinition**: Creates a *job definition* by using the specified HiveQL statements</span></span>
* <span data-ttu-id="a4e90-118">**Spuštění AzureRmHDInsightJob**: odešle definici úlohy do HDInsight, spustí úlohu a vrátí *úlohy* objekt, který můžete použít ke kontrole stavu úlohy</span><span class="sxs-lookup"><span data-stu-id="a4e90-118">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job</span></span>
* <span data-ttu-id="a4e90-119">**Počkejte AzureRmHDInsightJob**: používá objekt úlohy a zkontrolujte stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="a4e90-119">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="a4e90-120">Se čeká na dokončení úlohy nebo je překročena doba čekání.</span><span class="sxs-lookup"><span data-stu-id="a4e90-120">It waits until the job completes or the wait time is exceeded.</span></span>
* <span data-ttu-id="a4e90-121">**Get-AzureRmHDInsightJobOutput**: používá se k načtení výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="a4e90-121">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job</span></span>
* <span data-ttu-id="a4e90-122">**Vyvolání AzureRmHDInsightHiveJob**: používá ke spouštění příkazy HiveQL.</span><span class="sxs-lookup"><span data-stu-id="a4e90-122">**Invoke-AzureRmHDInsightHiveJob**: Used to run HiveQL statements.</span></span> <span data-ttu-id="a4e90-123">Tato rutina bloky dotaz dokončení a potom vrátí výsledky</span><span class="sxs-lookup"><span data-stu-id="a4e90-123">This cmdlet blocks the query completes, then returns the results</span></span>
* <span data-ttu-id="a4e90-124">**Použití AzureRmHDInsightCluster**: Nastaví aktuální cluster pro **Invoke-AzureRmHDInsightHiveJob** příkaz</span><span class="sxs-lookup"><span data-stu-id="a4e90-124">**Use-AzureRmHDInsightCluster**: Sets the current cluster to use for the **Invoke-AzureRmHDInsightHiveJob** command</span></span>

<span data-ttu-id="a4e90-125">Následující kroky ukazují, jak tyto rutiny použít ke spuštění úlohy v clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a4e90-125">The following steps demonstrate how to use these cmdlets to run a job in your HDInsight cluster:</span></span>

1. <span data-ttu-id="a4e90-126">Pomocí editoru, uložte následující kód jako **hivejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="a4e90-126">Using an editor, save the following code as **hivejob.ps1**.</span></span>

    <span data-ttu-id="a4e90-127">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]</span><span class="sxs-lookup"><span data-stu-id="a4e90-127">[!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]</span></span>

2. <span data-ttu-id="a4e90-128">Otevřete nový **prostředí Azure PowerShell** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="a4e90-128">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="a4e90-129">Změňte adresáře na umístění **hivejob.ps1** souboru a potom použijte následující příkaz pro spuštění skriptu:</span><span class="sxs-lookup"><span data-stu-id="a4e90-129">Change directories to the location of the **hivejob.ps1** file, then use the following command to run the script:</span></span>

        .\hivejob.ps1

    <span data-ttu-id="a4e90-130">Při spuštění skriptu, zobrazí se výzva k zadání názvu clusteru a přihlašovací údaje účtu HTTPS nebo Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="a4e90-130">When the script runs, you are prompted to enter the cluster name and the HTTPS/Admin account credentials for the cluster.</span></span> <span data-ttu-id="a4e90-131">Také můžete být vyzváni k přihlášení k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="a4e90-131">You may also be prompted to log in to your Azure subscription.</span></span>

3. <span data-ttu-id="a4e90-132">Po dokončení úlohy vrátí informace podobná následující thext:</span><span class="sxs-lookup"><span data-stu-id="a4e90-132">When the job completes, it returns information similar to the following thext:</span></span>

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. <span data-ttu-id="a4e90-133">Jak už bylo zmíněno dříve, **Invoke-Hive** lze použít ke spuštění dotazu a čekat na odpověď.</span><span class="sxs-lookup"><span data-stu-id="a4e90-133">As mentioned earlier, **Invoke-Hive** can be used to run a query and wait for the response.</span></span> <span data-ttu-id="a4e90-134">Chcete-li zjistit, jak funguje Invoke-Hive pomocí následujícího skriptu:</span><span class="sxs-lookup"><span data-stu-id="a4e90-134">Use the following script to see how Invoke-Hive works:</span></span>

    <span data-ttu-id="a4e90-135">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]</span><span class="sxs-lookup"><span data-stu-id="a4e90-135">[!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]</span></span>

    <span data-ttu-id="a4e90-136">Výstup vypadá následující text:</span><span class="sxs-lookup"><span data-stu-id="a4e90-136">The output looks like the following text:</span></span>

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > <span data-ttu-id="a4e90-137">Pro delší HiveQL dotazy, můžete použít prostředí Azure PowerShell **sem řetězce** rutina nebo HiveQL soubory skriptů.</span><span class="sxs-lookup"><span data-stu-id="a4e90-137">For longer HiveQL queries, you can use the Azure PowerShell **Here-Strings** cmdlet or HiveQL script files.</span></span> <span data-ttu-id="a4e90-138">Následující fragment kódu ukazuje způsob použití **Invoke-Hive** můžete spustit soubor skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="a4e90-138">The following snippet shows how to use the **Invoke-Hive** cmdlet to run a HiveQL script file.</span></span> <span data-ttu-id="a4e90-139">Soubor skriptu HiveQL musí být odeslán do wasb: / /.</span><span class="sxs-lookup"><span data-stu-id="a4e90-139">The HiveQL script file must be uploaded to wasb://.</span></span>
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > <span data-ttu-id="a4e90-140">Další informace o **sem řetězce**, najdete v části <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">pomocí Windows PowerShell zde-řetězce</a>.</span><span class="sxs-lookup"><span data-stu-id="a4e90-140">For more information about **Here-Strings**, see <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell Here-Strings</a>.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a4e90-141">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a4e90-141">Troubleshooting</span></span>

<span data-ttu-id="a4e90-142">Pokud žádné informace se vrátí po dokončení úlohy, mohlo dojít k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="a4e90-142">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="a4e90-143">Chcete-li zobrazit informace o chybě pro tuto úlohu, přidejte na konec následující **hivejob.ps1** souboru, uložit jej a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="a4e90-143">To view error information for this job, add the following to the end of the **hivejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="a4e90-144">Tato rutina vrátí informace, které se zapisují do STDERR na serveru při spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="a4e90-144">This cmdlet returns the information that is written to STDERR on the server when you ran the job.</span></span>

## <a name="summary"></a><span data-ttu-id="a4e90-145">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a4e90-145">Summary</span></span>

<span data-ttu-id="a4e90-146">Jak vidíte, Azure PowerShell poskytuje snadný způsob, jak spouštět dotazy Hive v clusteru služby HDInsight, monitorovat stav úlohy a načíst výstup.</span><span class="sxs-lookup"><span data-stu-id="a4e90-146">As you can see, Azure PowerShell provides an easy way to run Hive queries in an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4e90-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a4e90-147">Next steps</span></span>

<span data-ttu-id="a4e90-148">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a4e90-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="a4e90-149">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a4e90-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="a4e90-150">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a4e90-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="a4e90-151">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a4e90-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="a4e90-152">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a4e90-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
