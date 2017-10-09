---
title: "aaaInstall a použití Giraph na Hadoop clusterů v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toocustomize HDInsight, cluster s Giraph a toouse Giraph."
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
ms.openlocfilehash: bd473faca9d3c87c29d7566a18fc94211c50f059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="b2935-103">Nainstalovat a používat Giraph v clusterech HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="b2935-103">Install and use Giraph on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="b2935-104">Zjistěte, jak toocustomize Windows na základě clusteru HDInsight s Giraph pomocí akce skriptu a jak toouse Giraph tooprocess rozsáhlé grafy.</span><span class="sxs-lookup"><span data-stu-id="b2935-104">Learn how toocustomize Windows based HDInsight cluster with Giraph using Script Action, and how toouse Giraph tooprocess large-scale graphs.</span></span> <span data-ttu-id="b2935-105">Informace o používání Giraph s clusteru se systémem Linux najdete v tématu [Giraph nainstalovat na clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b2935-105">For information on using Giraph with a Linux-based cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b2935-106">Hello kroky v této dokumentu funguje jenom s clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="b2935-106">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="b2935-107">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="b2935-107">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="b2935-108">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b2935-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b2935-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b2935-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="b2935-110">Informace o tom najdete v části tooinstall Giraph na cluster HDInsight se systémem Linux [Giraph nainstalovat na clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b2935-110">For information on how tooinstall Giraph on a Linux-based HDInsight cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>


<span data-ttu-id="b2935-111">Giraph můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*.</span><span class="sxs-lookup"><span data-stu-id="b2935-111">You can install Giraph on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="b2935-112">Tooinstall skriptu ukázka Giraph v clusteru HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="b2935-112">A sample script tooinstall Giraph on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span> <span data-ttu-id="b2935-113">Ukázkový skript Hello pracuje pouze s verze clusteru HDInsight verze 3.1.</span><span class="sxs-lookup"><span data-stu-id="b2935-113">hello sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="b2935-114">Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="b2935-114">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="b2935-115">**Související články**</span><span class="sxs-lookup"><span data-stu-id="b2935-115">**Related articles**</span></span>

* [<span data-ttu-id="b2935-116">Nainstalujte Giraph clusterů systému HDInsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="b2935-116">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="b2935-117">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b2935-117">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="b2935-118">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="b2935-118">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="b2935-119">[Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="b2935-119">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-giraph"></a><span data-ttu-id="b2935-120">Co je Giraph?</span><span class="sxs-lookup"><span data-stu-id="b2935-120">What is Giraph?</span></span>
<span data-ttu-id="b2935-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> vám umožní tooperform grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b2935-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="b2935-122">Grafy modelování vztahů mezi objekty, například hello připojení mezi směrovači v velkým síťovým jako hello Internetu, nebo vztahy mezi uživatelé v sociálních sítích (někdy označují tooas sociálních grafu).</span><span class="sxs-lookup"><span data-stu-id="b2935-122">Graphs model relationships between objects, such as hello connections between routers on a large network like hello Internet, or relationships between people on social networks (sometimes referred tooas a social graph).</span></span> <span data-ttu-id="b2935-123">Graf zpracování vám umožní tooreason o hello vztahy mezi objekty v grafu, jako například:</span><span class="sxs-lookup"><span data-stu-id="b2935-123">Graph processing allows you tooreason about hello relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="b2935-124">Identifikace potenciální přátel na základě vaší aktuální relací.</span><span class="sxs-lookup"><span data-stu-id="b2935-124">Identifying potential friends based on your current relationships.</span></span>
* <span data-ttu-id="b2935-125">Identifikace hello nejkratší trasy mezi dvěma počítači v síti.</span><span class="sxs-lookup"><span data-stu-id="b2935-125">Identifying hello shortest route between two computers in a network.</span></span>
* <span data-ttu-id="b2935-126">Výpočet pořadí stránku hello webové stránky.</span><span class="sxs-lookup"><span data-stu-id="b2935-126">Calculating hello page rank of webpages.</span></span>

## <a name="install-giraph-using-portal"></a><span data-ttu-id="b2935-127">Nainstalujte Giraph pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="b2935-127">Install Giraph using portal</span></span>
1. <span data-ttu-id="b2935-128">Zahájení vytváření clusteru pomocí hello **vytvořit vlastní** možnost, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="b2935-128">Start creating a cluster by using hello **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="b2935-129">Na hello **akcí skriptů** stránku hello průvodce, klikněte na tlačítko **přidat akce skriptu** tooprovide podrobností o hello akce skriptu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="b2935-129">On hello **Script Actions** page of hello wizard, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="b2935-130">![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "použití akce skriptu toocustomize clusteru")</span><span class="sxs-lookup"><span data-stu-id="b2935-130">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="b2935-131">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b2935-131">Property</span></span></th><th><span data-ttu-id="b2935-132">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b2935-132">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="b2935-133">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b2935-133">Name</span></span></td>
            <td><span data-ttu-id="b2935-134">Zadejte název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="b2935-134">Specify a name for hello script action.</span></span> <span data-ttu-id="b2935-135">Například <b>nainstalovat Giraph</b>.</span><span class="sxs-lookup"><span data-stu-id="b2935-135">For example, <b>Install Giraph</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="b2935-136">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="b2935-136">Script URI</span></span></td>
            <td><span data-ttu-id="b2935-137">Zadejte hello identifikátor URI (Uniform Resource) toohello skript, který je vyvolaná toocustomize hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2935-137">Specify hello Uniform Resource Identifier (URI) toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="b2935-138">Například <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="b2935-138">For example, <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="b2935-139">Typ uzlu</span><span class="sxs-lookup"><span data-stu-id="b2935-139">Node Type</span></span></td>
            <td><span data-ttu-id="b2935-140">Zadejte hello uzly, na kterých běží hello přizpůsobení skriptu.</span><span class="sxs-lookup"><span data-stu-id="b2935-140">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="b2935-141">Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>pouze uzly pracovního procesu</b>.</span><span class="sxs-lookup"><span data-stu-id="b2935-141">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="b2935-142">Parametry</span><span class="sxs-lookup"><span data-stu-id="b2935-142">Parameters</span></span></td>
            <td><span data-ttu-id="b2935-143">Zadejte parametry hello, pokud to vyžaduje hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="b2935-143">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="b2935-144">Hello skriptu tooinstall Giraph nevyžaduje žádné parametry, takže to můžete nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="b2935-144">hello script tooinstall Giraph does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="b2935-145">Více součástí, které můžete přidat více než jeden tooinstall akce skriptu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b2935-145">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="b2935-146">Po přidání hello skripty, klikněte na tlačítko toostart zaškrtnutí hello vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b2935-146">After you have added hello scripts, click hello checkmark toostart creating hello cluster.</span></span>

## <a name="use-giraph"></a><span data-ttu-id="b2935-147">Použití Giraph</span><span class="sxs-lookup"><span data-stu-id="b2935-147">Use Giraph</span></span>
<span data-ttu-id="b2935-148">Používáme hello SimpleShortestPathsComputation příklad toodemonstrate hello basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementace pro hledání hello nejkratší cesta mezi objekty v grafu.</span><span class="sxs-lookup"><span data-stu-id="b2935-148">We use hello SimpleShortestPathsComputation example toodemonstrate hello basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementation for finding hello shortest path between objects in a graph.</span></span> <span data-ttu-id="b2935-149">Pomocí následujících kroků tooupload hello ukázkových dat a hello ukázka jar, spustit úlohu pomocí hello SimpleShortestPathsComputation příklad a zobrazení výsledků hello hello.</span><span class="sxs-lookup"><span data-stu-id="b2935-149">Use hello following steps tooupload hello sample data and hello sample jar, run a job by using hello SimpleShortestPathsComputation example, and then view hello results.</span></span>

1. <span data-ttu-id="b2935-150">Nahrajte ukázková data souboru tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="b2935-150">Upload a sample data file tooAzure Blob storage.</span></span> <span data-ttu-id="b2935-151">Na místní pracovní stanici, vytvořte nový soubor s názvem **tiny_graph.txt**.</span><span class="sxs-lookup"><span data-stu-id="b2935-151">On your local workstation, create a new file named **tiny_graph.txt**.</span></span> <span data-ttu-id="b2935-152">Měl by obsahovat hello následující řádky:</span><span class="sxs-lookup"><span data-stu-id="b2935-152">It should contain hello following lines:</span></span>

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    <span data-ttu-id="b2935-153">Nahrajte hello tiny_graph.txt souboru toohello primárního úložiště pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b2935-153">Upload hello tiny_graph.txt file toohello primary storage for your HDInsight cluster.</span></span> <span data-ttu-id="b2935-154">Pokyny najdete v části tooupload data [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="b2935-154">For instructions on how tooupload data, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    <span data-ttu-id="b2935-155">Tato data popisuje vztah mezi objekty v řízené grafu, ve formátu hello [zdroj\_id, zdroj\_hodnotu [[cíle\_id], [edge\_hodnotu],...]]. Každý řádek představuje vztah mezi **zdroj\_id** objektů a jeden nebo více **cíle\_id** objekty.</span><span class="sxs-lookup"><span data-stu-id="b2935-155">This data describes a relationship between objects in a directed graph, by using hello format [source\_id, source\_value,[[dest\_id], [edge\_value],...]]. Each line represents a relationship between a **source\_id** object and one or more **dest\_id** objects.</span></span> <span data-ttu-id="b2935-156">Hello **edge\_hodnotu** (nebo váhy) lze považovat za hello sílu nebo distance hello připojení mezi **source_id** a **cíle\_id**.</span><span class="sxs-lookup"><span data-stu-id="b2935-156">hello **edge\_value** (or weight) can be thought of as hello strength or distance of hello connection between **source_id** and **dest\_id**.</span></span>

    <span data-ttu-id="b2935-157">Vykreslovat limitu, a pomocí hodnoty hello (nebo váhy) jako hello vzdálenost mezi objekty, hello výše dat může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="b2935-157">Drawn out, and using hello value (or weight) as hello distance between objects, hello above data might look like this:</span></span>

    ![tiny_graph.txt vykreslovat jako kroužky řádků různých vzdálenost mezi](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. <span data-ttu-id="b2935-159">Spusťte příklad SimpleShortestPathsComputation hello.</span><span class="sxs-lookup"><span data-stu-id="b2935-159">Run hello SimpleShortestPathsComputation example.</span></span> <span data-ttu-id="b2935-160">Použijte následující příklad hello toorun rutiny Azure PowerShell pomocí souboru tiny_graph.txt hello jako vstup hello.</span><span class="sxs-lookup"><span data-stu-id="b2935-160">Use hello following Azure PowerShell cmdlets toorun hello example by using hello tiny_graph.txt file as input.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b2935-161">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="b2935-161">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="b2935-162">kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b2935-162">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="b2935-163">Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b2935-163">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="b2935-164">Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b2935-164">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

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
    # Create hello definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run hello job, write output toohello Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    <span data-ttu-id="b2935-165">V hello výše příklad, nahraďte **clustername** s názvem hello clusteru HDInsight s Giraph nainstalována.</span><span class="sxs-lookup"><span data-stu-id="b2935-165">In hello above example, replace **clustername** with hello name of your HDInsight cluster that has Giraph installed.</span></span>
3. <span data-ttu-id="b2935-166">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="b2935-166">View hello results.</span></span> <span data-ttu-id="b2935-167">Po dokončení úlohy hello hello výsledky se budou ukládat do dvou výstupní soubory v hello **wasb: / / / Příklad nebo na více systémů nebo shotestpaths** složky.</span><span class="sxs-lookup"><span data-stu-id="b2935-167">Once hello job has finished, hello results will be stored in two output files in hello **wasb:///example/out/shotestpaths** folder.</span></span> <span data-ttu-id="b2935-168">soubory Hello se nazývají **část m-00001** a **část m-00002**.</span><span class="sxs-lookup"><span data-stu-id="b2935-168">hello files are called **part-m-00001** and **part-m-00002**.</span></span> <span data-ttu-id="b2935-169">Proveďte následující kroky toodownload a zobrazení hello výstup hello:</span><span class="sxs-lookup"><span data-stu-id="b2935-169">Perform hello following steps toodownload and view hello output:</span></span>

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select hello current subscription
    Select-AzureSubscription $subscriptionName

    # Create hello Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download hello job output toohello workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    <span data-ttu-id="b2935-170">Tím se vytvoří hello **příklad nebo výstupní/shortestpaths** struktury adresářů v aktuálním adresáři hello na pracovní stanici a stažení hello dva výstupní soubory toothat umístění.</span><span class="sxs-lookup"><span data-stu-id="b2935-170">This will create hello **example/output/shortestpaths** directory structure in hello current directory on your workstation, and download hello two output files toothat location.</span></span>

    <span data-ttu-id="b2935-171">Použití hello **Cat** rutiny toodisplay hello obsah hello souborů:</span><span class="sxs-lookup"><span data-stu-id="b2935-171">Use hello **Cat** cmdlet toodisplay hello contents of hello files:</span></span>

        Cat example/output/shortestpaths/part*

    <span data-ttu-id="b2935-172">výstup Hello by měla vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="b2935-172">hello output should appear similar toohello following:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="b2935-173">Příklad SimpleShortestPathComputation Hello je naprogramováno toostart s objektem ID 1 a najít hello nejkratší cesta tooother objekty.</span><span class="sxs-lookup"><span data-stu-id="b2935-173">hello SimpleShortestPathComputation example is hard coded toostart with object ID 1 and find hello shortest path tooother objects.</span></span> <span data-ttu-id="b2935-174">Proto byste si měli přečíst výstup hello jako `destination_id distance`, kde je vzdálenost hello hodnota (nebo váhy) hello okrajů kterou urazit mezi objektem ID 1 a ID hello cíl.</span><span class="sxs-lookup"><span data-stu-id="b2935-174">So hello output should be read as `destination_id distance`, where distance is hello value (or weight) of hello edges traveled between object ID 1 and hello target ID.</span></span>

    <span data-ttu-id="b2935-175">Tato vizualizace, můžete ověřit výsledky hello pomocí přechodu cesty nejkratší hello mezi ID 1 a všechny ostatní objekty.</span><span class="sxs-lookup"><span data-stu-id="b2935-175">Visualizing this, you can verify hello results by traveling hello shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="b2935-176">Všimněte si, že hello nejkratší cesta ID 1 až ID 4 je 5.</span><span class="sxs-lookup"><span data-stu-id="b2935-176">Note that hello shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="b2935-177">Toto je celkový počet vzdálenost hello mezi <span style="color:orange">ID 1 a 3</span>a potom <span style="color:red">ID 3 a 4</span>.</span><span class="sxs-lookup"><span data-stu-id="b2935-177">This is hello total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Kreslení objektů jako kroužky s nejkratší cest mezi](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a><span data-ttu-id="b2935-179">Nainstalujte Giraph pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b2935-179">Install Giraph using Aure PowerShell</span></span>
<span data-ttu-id="b2935-180">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="b2935-180">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="b2935-181">Ukázka Hello ukazuje, jak tooinstall Spark pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b2935-181">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="b2935-182">Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="b2935-182">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="install-giraph-using-net-sdk"></a><span data-ttu-id="b2935-183">Nainstalujte Giraph pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b2935-183">Install Giraph using .NET SDK</span></span>
<span data-ttu-id="b2935-184">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="b2935-184">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="b2935-185">Ukázka Hello ukazuje, jak tooinstall Spark pomocí hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="b2935-185">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="b2935-186">Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="b2935-186">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="b2935-187">Viz také</span><span class="sxs-lookup"><span data-stu-id="b2935-187">See also</span></span>
* [<span data-ttu-id="b2935-188">Nainstalujte Giraph clusterů systému HDInsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="b2935-188">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="b2935-189">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b2935-189">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="b2935-190">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="b2935-190">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="b2935-191">[Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="b2935-191">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="b2935-192">[Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark.</span><span class="sxs-lookup"><span data-stu-id="b2935-192">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="b2935-193">[Nainstalujte jazyk R v clusterech HDInsight][hdinsight-install-r]: akce skriptu vzorku o instalaci R.</span><span class="sxs-lookup"><span data-stu-id="b2935-193">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="b2935-194">[Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install.md): akce skriptu vzorku o instalaci Solr.</span><span class="sxs-lookup"><span data-stu-id="b2935-194">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md): Script Action sample about installing Solr.</span></span>

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
