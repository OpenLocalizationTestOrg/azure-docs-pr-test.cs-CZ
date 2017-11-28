---
title: "aaaUse Hadoop Hive v prostředí PowerShell v HDInsight - Azure | Microsoft Docs"
description: "Pomocí dotazů Hive toorun prostředí PowerShell v Hadoop v HDInsight."
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
ms.openlocfilehash: 9e0b72a25c5b12431f837b1a34a63ecc06223528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-powershell"></a><span data-ttu-id="b1dcf-103">Spouštění dotazů Hive pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b1dcf-103">Run Hive queries using PowerShell</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="b1dcf-104">Tento dokument poskytuje příklad použití Azure PowerShell v dotazy Hive hello skupiny prostředků Azure režimu toorun v Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-104">This document provides an example of using Azure PowerShell in hello Azure Resource Group mode toorun Hive queries in a Hadoop on HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="b1dcf-105">Tento dokument neposkytuje podrobný popis co dělat hello HiveQL příkazy, které se používají v příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-105">This document does not provide a detailed description of what hello HiveQL statements that are used in hello examples do.</span></span> <span data-ttu-id="b1dcf-106">Informace o hello HiveQL, který se používá v tomto příkladu najdete v tématu [používání Hive s Hadoop v HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="b1dcf-106">For information on hello HiveQL that is used in this example, see [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="b1dcf-107">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="b1dcf-107">**Prerequisites**</span></span>

* <span data-ttu-id="b1dcf-108">**Cluster Azure HDInsight**: nezáleží na tom, jestli je Windows hello clusteru nebo systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-108">**An Azure HDInsight cluster**: It does not matter whether hello cluster is Windows or Linux-based.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="b1dcf-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b1dcf-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b1dcf-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="b1dcf-111">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-111">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a><span data-ttu-id="b1dcf-112">Spouštění dotazů Hive pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b1dcf-112">Run Hive queries using Azure PowerShell</span></span>

<span data-ttu-id="b1dcf-113">Prostředí Azure PowerShell poskytuje *rutiny* , umožňují tooremotely spouštění dotazů Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-113">Azure PowerShell provides *cmdlets* that allow you tooremotely run Hive queries on HDInsight.</span></span> <span data-ttu-id="b1dcf-114">Interně hello rutiny volání REST příliš[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) na clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-114">Internally, hello cmdlets make REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on hello HDInsight cluster.</span></span>

<span data-ttu-id="b1dcf-115">Hello se používají následující rutiny při spuštění dotazů Hive v clusteru s podporou vzdálené HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-115">hello following cmdlets are used when running Hive queries in a remote HDInsight cluster:</span></span>

* <span data-ttu-id="b1dcf-116">**Přidat-AzureRmAccount**: ověřuje Azure PowerShell tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="b1dcf-116">**Add-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure subscription</span></span>
* <span data-ttu-id="b1dcf-117">**Nové AzureRmHDInsightHiveJobDefinition**: vytvoří *úlohy definice* pomocí hello zadaný příkazy HiveQL</span><span class="sxs-lookup"><span data-stu-id="b1dcf-117">**New-AzureRmHDInsightHiveJobDefinition**: Creates a *job definition* by using hello specified HiveQL statements</span></span>
* <span data-ttu-id="b1dcf-118">**Spuštění AzureRmHDInsightJob**: odešle tooHDInsight definice úlohy hello, spustí hello úlohy a vrátí *úlohy* objekt, který lze použít toocheck hello stav úlohy hello</span><span class="sxs-lookup"><span data-stu-id="b1dcf-118">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job</span></span>
* <span data-ttu-id="b1dcf-119">**Počkejte AzureRmHDInsightJob**: používá objekt úlohy hello toocheck hello stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-119">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="b1dcf-120">Se čeká na dokončení úlohy hello nebo je překročena doba čekání hello.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-120">It waits until hello job completes or hello wait time is exceeded.</span></span>
* <span data-ttu-id="b1dcf-121">**Get-AzureRmHDInsightJobOutput**: používá tooretrieve hello výstup hello úlohy</span><span class="sxs-lookup"><span data-stu-id="b1dcf-121">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job</span></span>
* <span data-ttu-id="b1dcf-122">**Vyvolání AzureRmHDInsightHiveJob**: používat příkazy HiveQL toorun.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-122">**Invoke-AzureRmHDInsightHiveJob**: Used toorun HiveQL statements.</span></span> <span data-ttu-id="b1dcf-123">Tento dotaz hello bloky rutiny dokončení a potom vrátí výsledky hello</span><span class="sxs-lookup"><span data-stu-id="b1dcf-123">This cmdlet blocks hello query completes, then returns hello results</span></span>
* <span data-ttu-id="b1dcf-124">**Použití AzureRmHDInsightCluster**: Nastaví hello aktuální toouse clusteru pro hello **Invoke-AzureRmHDInsightHiveJob** příkaz</span><span class="sxs-lookup"><span data-stu-id="b1dcf-124">**Use-AzureRmHDInsightCluster**: Sets hello current cluster toouse for hello **Invoke-AzureRmHDInsightHiveJob** command</span></span>

<span data-ttu-id="b1dcf-125">Hello následující kroky ukazují, jak toouse tyto rutiny toorun úlohu v clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-125">hello following steps demonstrate how toouse these cmdlets toorun a job in your HDInsight cluster:</span></span>

1. <span data-ttu-id="b1dcf-126">Pomocí editoru, uložte hello následující kód jako **hivejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-126">Using an editor, save hello following code as **hivejob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. <span data-ttu-id="b1dcf-127">Otevřete nový **prostředí Azure PowerShell** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-127">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="b1dcf-128">Umístění adresáře toohello hello změnit **hivejob.ps1** souboru, potom použijte následující příkaz toorun hello skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-128">Change directories toohello location of hello **hivejob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\hivejob.ps1

    <span data-ttu-id="b1dcf-129">Při spuštění skriptu hello jste výzvami tooenter hello clusteru název a hello HTTPS nebo přihlašovací údaje účtu správce pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-129">When hello script runs, you are prompted tooenter hello cluster name and hello HTTPS/Admin account credentials for hello cluster.</span></span> <span data-ttu-id="b1dcf-130">Může být také výzvami toolog v tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-130">You may also be prompted toolog in tooyour Azure subscription.</span></span>

3. <span data-ttu-id="b1dcf-131">Po dokončení úlohy hello vrátí následující thext podobné toohello informace:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-131">When hello job completes, it returns information similar toohello following thext:</span></span>

        Display hello standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. <span data-ttu-id="b1dcf-132">Jak už bylo zmíněno dříve, **Invoke-Hive** lze použít toorun dotazu a čeká na odpověď hello.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-132">As mentioned earlier, **Invoke-Hive** can be used toorun a query and wait for hello response.</span></span> <span data-ttu-id="b1dcf-133">Použijte následující skript toosee fungování Invoke-Hive hello:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-133">Use hello following script toosee how Invoke-Hive works:</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    <span data-ttu-id="b1dcf-134">výstup Hello vypadá hello následující text:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-134">hello output looks like hello following text:</span></span>

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > <span data-ttu-id="b1dcf-135">Pro delší HiveQL dotazy, můžete použít hello prostředí Azure PowerShell **sem řetězce** rutina nebo HiveQL soubory skriptů.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-135">For longer HiveQL queries, you can use hello Azure PowerShell **Here-Strings** cmdlet or HiveQL script files.</span></span> <span data-ttu-id="b1dcf-136">Následující fragment kódu ukazuje, jak Hello toouse hello **Invoke-Hive** rutiny toorun soubor skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-136">hello following snippet shows how toouse hello **Invoke-Hive** cmdlet toorun a HiveQL script file.</span></span> <span data-ttu-id="b1dcf-137">Hello HiveQL souboru skriptu musí být nahrán toowasb: / /.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-137">hello HiveQL script file must be uploaded toowasb://.</span></span>
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > <span data-ttu-id="b1dcf-138">Další informace o **sem řetězce**, najdete v části <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">pomocí Windows PowerShell zde-řetězce</a>.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-138">For more information about **Here-Strings**, see <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell Here-Strings</a>.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b1dcf-139">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b1dcf-139">Troubleshooting</span></span>

<span data-ttu-id="b1dcf-140">Když se po dokončení úlohy hello nevrátí žádné informace, může mít došlo k chybě během zpracování.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-140">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="b1dcf-141">informace o chybě tooview pro tuto úlohu, přidejte následující toohello konec hello hello **hivejob.ps1** souboru, uložit jej a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-141">tooview error information for this job, add hello following toohello end of hello **hivejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print hello output of hello Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="b1dcf-142">Tato rutina vrátí hello informace, které se zapisují tooSTDERR na serveru hello při spuštění úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-142">This cmdlet returns hello information that is written tooSTDERR on hello server when you ran hello job.</span></span>

## <a name="summary"></a><span data-ttu-id="b1dcf-143">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b1dcf-143">Summary</span></span>

<span data-ttu-id="b1dcf-144">Jak vidíte, prostředí Azure PowerShell poskytuje snadný způsob toorun dotazů Hive v clusteru služby HDInsight, monitorování hello stav úlohy a načíst výstup hello.</span><span class="sxs-lookup"><span data-stu-id="b1dcf-144">As you can see, Azure PowerShell provides an easy way toorun Hive queries in an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b1dcf-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1dcf-145">Next steps</span></span>

<span data-ttu-id="b1dcf-146">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-146">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="b1dcf-147">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b1dcf-147">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="b1dcf-148">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b1dcf-148">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="b1dcf-149">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b1dcf-149">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="b1dcf-150">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b1dcf-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
