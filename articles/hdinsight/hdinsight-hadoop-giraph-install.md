---
title: "Na nainstalovat a používat Giraph clusterů systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit clusteru HDInsight s Giraph a jak používat Giraph."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f0eb5c1f457380600463a370043f03e6d655a02c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="ed749-103">Nainstalovat a používat Giraph v clusterech HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="ed749-103">Install and use Giraph on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="ed749-104">Zjistěte, jak přizpůsobit na bázi clusteru HDInsight se systémem Windows s Giraph pomocí akce skriptu a postupy pro použití Giraph ke zpracování velkých grafy.</span><span class="sxs-lookup"><span data-stu-id="ed749-104">Learn how to customize Windows based HDInsight cluster with Giraph using Script Action, and how to use Giraph to process large-scale graphs.</span></span> <span data-ttu-id="ed749-105">Informace o používání Giraph s clusteru se systémem Linux najdete v tématu [Giraph nainstalovat na clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-105">For information on using Giraph with a Linux-based cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ed749-106">Kroky v tomto dokumentu pracovat pouze s clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ed749-106">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="ed749-107">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="ed749-107">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="ed749-108">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ed749-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ed749-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ed749-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="ed749-110">Informace o tom, jak nainstalovat Giraph na clusteru HDInsight se systémem Linux najdete v tématu [Giraph nainstalovat na clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-110">For information on how to install Giraph on a Linux-based HDInsight cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>


<span data-ttu-id="ed749-111">Giraph můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*.</span><span class="sxs-lookup"><span data-stu-id="ed749-111">You can install Giraph on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="ed749-112">Ukázkový skript pro instalaci Giraph v clusteru HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="ed749-112">A sample script to install Giraph on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span> <span data-ttu-id="ed749-113">Ukázkový skript pracuje pouze s verze clusteru HDInsight verze 3.1.</span><span class="sxs-lookup"><span data-stu-id="ed749-113">The sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="ed749-114">Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-114">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="ed749-115">**Související články**</span><span class="sxs-lookup"><span data-stu-id="ed749-115">**Related articles**</span></span>

* [<span data-ttu-id="ed749-116">Nainstalujte Giraph clusterů systému HDInsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="ed749-116">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="ed749-117">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed749-117">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="ed749-118">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="ed749-118">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="ed749-119">[Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-119">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-giraph"></a><span data-ttu-id="ed749-120">Co je Giraph?</span><span class="sxs-lookup"><span data-stu-id="ed749-120">What is Giraph?</span></span>
<span data-ttu-id="ed749-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> umožňuje provádět grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed749-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="ed749-122">Grafy model relace mezi objekty, jako je připojení mezi směrovače na velké sítě, jako je Internet nebo vztahy mezi uživateli v sociálních sítích (někdy označované jako sociálních grafu).</span><span class="sxs-lookup"><span data-stu-id="ed749-122">Graphs model relationships between objects, such as the connections between routers on a large network like the Internet, or relationships between people on social networks (sometimes referred to as a social graph).</span></span> <span data-ttu-id="ed749-123">Zpracování graf umožňuje důvod o vztahy mezi objekty v grafu, jako například:</span><span class="sxs-lookup"><span data-stu-id="ed749-123">Graph processing allows you to reason about the relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="ed749-124">Identifikace potenciální přátel na základě vaší aktuální relací.</span><span class="sxs-lookup"><span data-stu-id="ed749-124">Identifying potential friends based on your current relationships.</span></span>
* <span data-ttu-id="ed749-125">Identifikace nejkratší trasy mezi dvěma počítači v síti.</span><span class="sxs-lookup"><span data-stu-id="ed749-125">Identifying the shortest route between two computers in a network.</span></span>
* <span data-ttu-id="ed749-126">Výpočet pořadí stránky webových stránek.</span><span class="sxs-lookup"><span data-stu-id="ed749-126">Calculating the page rank of webpages.</span></span>

## <a name="install-giraph-using-portal"></a><span data-ttu-id="ed749-127">Nainstalujte Giraph pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="ed749-127">Install Giraph using portal</span></span>
1. <span data-ttu-id="ed749-128">Zahájení vytváření clusteru pomocí **vytvořit vlastní** možnost, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-128">Start creating a cluster by using the **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="ed749-129">Na **akcí skriptů** stránku průvodce, klikněte na tlačítko **přidat akce skriptu** poskytnout podrobnosti o akce skriptu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="ed749-129">On the **Script Actions** page of the wizard, click **add script action** to provide details about the script action, as shown below:</span></span>

    <span data-ttu-id="ed749-130">![Použití akce skriptu k přizpůsobení cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "použití akce skriptu k přizpůsobení clusteru")</span><span class="sxs-lookup"><span data-stu-id="ed749-130">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="ed749-131">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ed749-131">Property</span></span></th><th><span data-ttu-id="ed749-132">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ed749-132">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="ed749-133">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ed749-133">Name</span></span></td>
            <td><span data-ttu-id="ed749-134">Zadejte název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="ed749-134">Specify a name for the script action.</span></span> <span data-ttu-id="ed749-135">Například <b>nainstalovat Giraph</b>.</span><span class="sxs-lookup"><span data-stu-id="ed749-135">For example, <b>Install Giraph</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="ed749-136">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="ed749-136">Script URI</span></span></td>
            <td><span data-ttu-id="ed749-137">Zadejte identifikátoru URI (Uniform Resource) do skriptu, která je volána, chcete-li přizpůsobit clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed749-137">Specify the Uniform Resource Identifier (URI) to the script that is invoked to customize the cluster.</span></span> <span data-ttu-id="ed749-138">Například <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="ed749-138">For example, <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="ed749-139">Typ uzlu</span><span class="sxs-lookup"><span data-stu-id="ed749-139">Node Type</span></span></td>
            <td><span data-ttu-id="ed749-140">Zadejte uzly, na kterých běží přizpůsobení skriptu.</span><span class="sxs-lookup"><span data-stu-id="ed749-140">Specify the nodes on which the customization script is run.</span></span> <span data-ttu-id="ed749-141">Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>pouze uzly pracovního procesu</b>.</span><span class="sxs-lookup"><span data-stu-id="ed749-141">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="ed749-142">Parametry</span><span class="sxs-lookup"><span data-stu-id="ed749-142">Parameters</span></span></td>
            <td><span data-ttu-id="ed749-143">Zadejte parametry, pokud se vyžadují skriptem.</span><span class="sxs-lookup"><span data-stu-id="ed749-143">Specify the parameters, if required by the script.</span></span> <span data-ttu-id="ed749-144">Skript, který chcete nainstalovat Giraph nevyžaduje žádné parametry, takže to můžete nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="ed749-144">The script to install Giraph does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="ed749-145">Můžete přidat více než jednu akci skriptu pro instalaci více součástí v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed749-145">You can add more than one script action to install multiple components on the cluster.</span></span> <span data-ttu-id="ed749-146">Po přidání skripty, klikněte na značku zaškrtnutí zahájíte vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed749-146">After you have added the scripts, click the checkmark to start creating the cluster.</span></span>

## <a name="use-giraph"></a><span data-ttu-id="ed749-147">Použití Giraph</span><span class="sxs-lookup"><span data-stu-id="ed749-147">Use Giraph</span></span>
<span data-ttu-id="ed749-148">Příklad SimpleShortestPathsComputation používáme k předvedení základní <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementace pro vyhledání nejkratšího cesta mezi objekty v grafu.</span><span class="sxs-lookup"><span data-stu-id="ed749-148">We use the SimpleShortestPathsComputation example to demonstrate the basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementation for finding the shortest path between objects in a graph.</span></span> <span data-ttu-id="ed749-149">Použijte následující postup k odeslání vzorovými daty a ukázkové jar, spusťte úlohu pomocí SimpleShortestPathsComputation příklad a pak zobrazte výsledky.</span><span class="sxs-lookup"><span data-stu-id="ed749-149">Use the following steps to upload the sample data and the sample jar, run a job by using the SimpleShortestPathsComputation example, and then view the results.</span></span>

1. <span data-ttu-id="ed749-150">Ukázkový datový soubor nahrajte do Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="ed749-150">Upload a sample data file to Azure Blob storage.</span></span> <span data-ttu-id="ed749-151">Na místní pracovní stanici, vytvořte nový soubor s názvem **tiny_graph.txt**.</span><span class="sxs-lookup"><span data-stu-id="ed749-151">On your local workstation, create a new file named **tiny_graph.txt**.</span></span> <span data-ttu-id="ed749-152">Měl by obsahovat následující řádky:</span><span class="sxs-lookup"><span data-stu-id="ed749-152">It should contain the following lines:</span></span>

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    <span data-ttu-id="ed749-153">Nahrajte soubor tiny_graph.txt do primárního úložiště pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed749-153">Upload the tiny_graph.txt file to the primary storage for your HDInsight cluster.</span></span> <span data-ttu-id="ed749-154">Pokyny, jak odesílat data, najdete v části [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-154">For instructions on how to upload data, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    <span data-ttu-id="ed749-155">Tato data popisuje vztah mezi objekty v řízené grafu, ve formátu [zdroj\_id, zdroj\_hodnotu [[cíle\_id], [edge\_hodnotu],...]].</span><span class="sxs-lookup"><span data-stu-id="ed749-155">This data describes a relationship between objects in a directed graph, by using the format [source\_id, source\_value,[[dest\_id], [edge\_value],...]].</span></span> <span data-ttu-id="ed749-156">Každý řádek představuje vztah mezi **zdroj\_id** objektů a jeden nebo více **cíle\_id** objekty.</span><span class="sxs-lookup"><span data-stu-id="ed749-156">Each line represents a relationship between a **source\_id** object and one or more **dest\_id** objects.</span></span> <span data-ttu-id="ed749-157">**Edge\_hodnotu** (nebo váhy) lze považovat za sílu nebo distance připojení mezi **source_id** a **cíle\_id**.</span><span class="sxs-lookup"><span data-stu-id="ed749-157">The **edge\_value** (or weight) can be thought of as the strength or distance of the connection between **source_id** and **dest\_id**.</span></span>

    <span data-ttu-id="ed749-158">Vykreslovat limitu, a pomocí hodnoty (nebo váhy) jako vzdálenost mezi objekty, výše uvedená data může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="ed749-158">Drawn out, and using the value (or weight) as the distance between objects, the above data might look like this:</span></span>

    ![tiny_graph.txt vykreslovat jako kroužky řádků různých vzdálenost mezi](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. <span data-ttu-id="ed749-160">Spusťte SimpleShortestPathsComputation příklad.</span><span class="sxs-lookup"><span data-stu-id="ed749-160">Run the SimpleShortestPathsComputation example.</span></span> <span data-ttu-id="ed749-161">Pomocí následující rutiny prostředí Azure PowerShell pro spuštění příkladu pomocí souboru tiny_graph.txt jako vstup.</span><span class="sxs-lookup"><span data-stu-id="ed749-161">Use the following Azure PowerShell cmdlets to run the example by using the tiny_graph.txt file as input.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ed749-162">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="ed749-162">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="ed749-163">Kroky v tomto dokumentu používají nové rutiny služby HDInsight, které pracují s Azure Resource Managerem.</span><span class="sxs-lookup"><span data-stu-id="ed749-163">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="ed749-164">Podle postupu v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) si nainstalujte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ed749-164">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="ed749-165">Pokud máte skripty, které je potřeba upravit tak, aby používaly nové rutiny, které pracují s nástrojem Azure Resource Manager, najdete další informace v tématu [Migrace na vývojové nástroje založené na Azure Resource Manageru pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-165">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create the definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run the job, write output to the Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    <span data-ttu-id="ed749-166">Ve výše uvedeném příkladu nahraďte **clustername** s názvem clusteru HDInsight s Giraph nainstalována.</span><span class="sxs-lookup"><span data-stu-id="ed749-166">In the above example, replace **clustername** with the name of your HDInsight cluster that has Giraph installed.</span></span>
3. <span data-ttu-id="ed749-167">Zkontrolujte výsledky.</span><span class="sxs-lookup"><span data-stu-id="ed749-167">View the results.</span></span> <span data-ttu-id="ed749-168">Po dokončení úlohy, výsledky se uloží v dva výstupní soubory v **wasb: / / / Příklad nebo na více systémů nebo shotestpaths** složky.</span><span class="sxs-lookup"><span data-stu-id="ed749-168">Once the job has finished, the results will be stored in two output files in the **wasb:///example/out/shotestpaths** folder.</span></span> <span data-ttu-id="ed749-169">Soubory se nazývají **část m-00001** a **část m-00002**.</span><span class="sxs-lookup"><span data-stu-id="ed749-169">The files are called **part-m-00001** and **part-m-00002**.</span></span> <span data-ttu-id="ed749-170">Proveďte následující kroky ke stažení a zobrazte výstup:</span><span class="sxs-lookup"><span data-stu-id="ed749-170">Perform the following steps to download and view the output:</span></span>

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select the current subscription
    Select-AzureSubscription $subscriptionName

    # Create the Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download the job output to the workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    <span data-ttu-id="ed749-171">Tím se vytvoří **příklad nebo výstupní/shortestpaths** struktury adresářů v aktuálním adresáři na pracovní stanici a stažení dva výstupní soubory do tohoto umístění.</span><span class="sxs-lookup"><span data-stu-id="ed749-171">This will create the **example/output/shortestpaths** directory structure in the current directory on your workstation, and download the two output files to that location.</span></span>

    <span data-ttu-id="ed749-172">Použití **Cat** rutiny k zobrazení obsahu souborů:</span><span class="sxs-lookup"><span data-stu-id="ed749-172">Use the **Cat** cmdlet to display the contents of the files:</span></span>

        Cat example/output/shortestpaths/part*

    <span data-ttu-id="ed749-173">Výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="ed749-173">The output should appear similar to the following:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="ed749-174">SimpleShortestPathComputation příklad je pevný programového začínat objektu ID 1 a najít nejkratší cestu k jiné objekty.</span><span class="sxs-lookup"><span data-stu-id="ed749-174">The SimpleShortestPathComputation example is hard coded to start with object ID 1 and find the shortest path to other objects.</span></span> <span data-ttu-id="ed749-175">Proto byste si měli přečíst výstup jako `destination_id distance`, kde je vzdálenost hodnota (nebo váhy) okraje mezi objektem ID 1 a identifikátor cílové cesty</span><span class="sxs-lookup"><span data-stu-id="ed749-175">So the output should be read as `destination_id distance`, where distance is the value (or weight) of the edges traveled between object ID 1 and the target ID.</span></span>

    <span data-ttu-id="ed749-176">Tato vizualizace, můžete ověřit výsledky podle cestě nejkratší cest mezi ID 1 a všechny ostatní objekty.</span><span class="sxs-lookup"><span data-stu-id="ed749-176">Visualizing this, you can verify the results by traveling the shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="ed749-177">Všimněte si, že nejkratší cesta ID 1 až ID 4 je 5.</span><span class="sxs-lookup"><span data-stu-id="ed749-177">Note that the shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="ed749-178">Toto je celkový počet vzdálenost mezi <span style="color:orange">ID 1 a 3</span>a potom <span style="color:red">ID 3 a 4</span>.</span><span class="sxs-lookup"><span data-stu-id="ed749-178">This is the total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Kreslení objektů jako kroužky s nejkratší cest mezi](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a><span data-ttu-id="ed749-180">Nainstalujte Giraph pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ed749-180">Install Giraph using Aure PowerShell</span></span>
<span data-ttu-id="ed749-181">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="ed749-181">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="ed749-182">Ukázka ukazuje, jak nainstalovat Spark pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ed749-182">The sample demonstrates how to install Spark using Azure PowerShell.</span></span> <span data-ttu-id="ed749-183">Je nutné přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="ed749-183">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="install-giraph-using-net-sdk"></a><span data-ttu-id="ed749-184">Nainstalujte Giraph pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ed749-184">Install Giraph using .NET SDK</span></span>
<span data-ttu-id="ed749-185">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="ed749-185">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="ed749-186">Ukázka ukazuje, jak nainstalovat Spark pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="ed749-186">The sample demonstrates how to install Spark using the .NET SDK.</span></span> <span data-ttu-id="ed749-187">Je nutné přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="ed749-187">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="ed749-188">Viz také</span><span class="sxs-lookup"><span data-stu-id="ed749-188">See also</span></span>
* [<span data-ttu-id="ed749-189">Nainstalujte Giraph clusterů systému HDInsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="ed749-189">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="ed749-190">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed749-190">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="ed749-191">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="ed749-191">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="ed749-192">[Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="ed749-192">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="ed749-193">[Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark.</span><span class="sxs-lookup"><span data-stu-id="ed749-193">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="ed749-194">[Nainstalujte jazyk R v clusterech HDInsight][hdinsight-install-r]: akce skriptu vzorku o instalaci R.</span><span class="sxs-lookup"><span data-stu-id="ed749-194">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="ed749-195">[Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install.md): akce skriptu vzorku o instalaci Solr.</span><span class="sxs-lookup"><span data-stu-id="ed749-195">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md): Script Action sample about installing Solr.</span></span>

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
