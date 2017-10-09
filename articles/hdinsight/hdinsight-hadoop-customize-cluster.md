---
title: "aaaCustomize clusterů HDInsight pomocí skriptu akce - Azure | Microsoft Docs"
description: "Zjistěte, jak toocustomize HDInsight clustery pomocí akce skriptu."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 076fff23e016db47bc7e9963582a545ad638e691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="c059f-103">Přizpůsobení clusterů HDInsight se systémem Windows pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="c059f-103">Customize Windows-based HDInsight clusters using Script Action</span></span>
<span data-ttu-id="c059f-104">**Skript akce** lze použít tooinvoke [vlastní skripty](hdinsight-hadoop-script-actions.md) během procesu vytváření clusteru hello k instalaci na clusteru s podporou další software.</span><span class="sxs-lookup"><span data-stu-id="c059f-104">**Script Action** can be used tooinvoke [custom scripts](hdinsight-hadoop-script-actions.md) during hello cluster creation process for installing additional software on a cluster.</span></span>

<span data-ttu-id="c059f-105">Hello informace v tomto článku je konkrétní na základě tooWindows clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c059f-105">hello information in this article is specific tooWindows-based HDInsight clusters.</span></span> <span data-ttu-id="c059f-106">V clusterech se systémem Linux naleznete v části [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c059f-106">For Linux-based clusters, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c059f-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c059f-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c059f-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c059f-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="c059f-109">Clustery prostředí HDInsight lze přizpůsobit v různých další způsoby, například včetně další účty Azure Storage, změna hello Hadoop soubory konfigurace (core-site.xml, hive-site.xml atd.) nebo přidání sdílené knihovny (například Hive, Oozie) do společné umístění v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-109">HDInsight clusters can be customized in a variety of other ways as well, such as including additional Azure Storage accounts, changing hello Hadoop configuration files (core-site.xml, hive-site.xml, etc.), or adding shared libraries (e.g., Hive, Oozie) into common locations in hello cluster.</span></span> <span data-ttu-id="c059f-110">Toto vlastní nastavení můžete provést pomocí prostředí Azure PowerShell hello .NET SDK služby Azure HDInsight, nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c059f-110">These customizations can be done through Azure PowerShell, hello Azure HDInsight .NET SDK, or hello Azure portal.</span></span> <span data-ttu-id="c059f-111">Další informace najdete v tématu [vytvoření Hadoop clusterů v HDInsight][hdinsight-provision-cluster].</span><span class="sxs-lookup"><span data-stu-id="c059f-111">For more information, see [Create Hadoop clusters in HDInsight][hdinsight-provision-cluster].</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-hello-cluster-creation-process"></a><span data-ttu-id="c059f-112">Akce skriptu v procesu vytváření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="c059f-112">Script Action in hello cluster creation process</span></span>
<span data-ttu-id="c059f-113">Akce skriptu se používá pouze během hello proces vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c059f-113">Script Action is only used while a cluster is in hello process of being created.</span></span> <span data-ttu-id="c059f-114">Hello následující diagram znázorňuje, pokud je během procesu vytváření hello provést akce skriptu:</span><span class="sxs-lookup"><span data-stu-id="c059f-114">hello following diagram illustrates when Script Action is executed during hello creation process:</span></span>

<span data-ttu-id="c059f-115">![Přizpůsobení cluster HDInsight a fáze při vytváření clusteru][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="c059f-115">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="c059f-116">Po spuštění skriptu hello hello cluster zadá hello **ClusterCustomization** fáze.</span><span class="sxs-lookup"><span data-stu-id="c059f-116">When hello script is running, hello cluster enters hello **ClusterCustomization** stage.</span></span> <span data-ttu-id="c059f-117">V této fázi je spuštěna pod účtem správce systému hello hello skriptu, paralelně na všechny hello zadaný uzly v clusteru hello a poskytuje úplný správce oprávnění na uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-117">At this stage, hello script is run under hello system admin account, in parallel on all hello specified nodes in hello cluster, and provides full admin privileges on hello nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="c059f-118">Vzhledem k tomu, že máte oprávnění správce na uzlech clusteru hello během **ClusterCustomization** fáze, můžete použít hello skriptu tooperform operací, jako je spouštění a zastavování služeb, včetně služby související s Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c059f-118">Because you have admin privileges on hello cluster nodes during the **ClusterCustomization** stage, you can use hello script tooperform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="c059f-119">Proto v rámci skriptu hello, musíte zajistit této služby hello Ambari a další služby související s Hadoop jsou spuštěny před hello skriptu dokončení spuštění.</span><span class="sxs-lookup"><span data-stu-id="c059f-119">So, as part of hello script, you must ensure that hello Ambari services and other Hadoop-related services are up and running before hello script finishes running.</span></span> <span data-ttu-id="c059f-120">Tyto služby jsou potřeba toosuccessfully zjistit hello stav a stav hello clusteru při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="c059f-120">These services are required toosuccessfully ascertain hello health and state of hello cluster while it is being created.</span></span> <span data-ttu-id="c059f-121">Pokud změníte žádnou konfiguraci v clusteru, který má vliv na tyto služby, musíte použít hello pomocných funkcí, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c059f-121">If you change any configuration on the cluster that affects these services, you must use hello helper functions that are provided.</span></span> <span data-ttu-id="c059f-122">Další informace o pomocných funkcí najdete v tématu [vyvíjet akce skriptu skripty pro HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="c059f-122">For more information about helper functions, see [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>
>
>

<span data-ttu-id="c059f-123">Hello výstup a protokoly chyb hello hello skriptu jsou uloženy v hello výchozí účet úložiště, které jste zadali pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="c059f-123">hello output and hello error logs for hello script are stored in hello default Storage account you specified for hello cluster.</span></span> <span data-ttu-id="c059f-124">Hello protokoly se ukládají v tabulce s názvem hello **u < \cluster-name-fragment >< \time-stamp > setuplog**.</span><span class="sxs-lookup"><span data-stu-id="c059f-124">hello logs are stored in a table with hello name **u<\cluster-name-fragment><\time-stamp>setuplog**.</span></span> <span data-ttu-id="c059f-125">Toto jsou agregovaná protokoly ze skriptu hello spouštět na všech uzlech hello (hlavního uzlu a pracovní uzly) v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-125">These are aggregate logs from hello script run on all hello nodes (head node and worker nodes) in hello cluster.</span></span>

<span data-ttu-id="c059f-126">Každý cluster může přijmout víc akcí skriptů, které jsou vyvolány v hello pořadí, ve kterém jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="c059f-126">Each cluster can accept multiple script actions that are invoked in hello order in which they are specified.</span></span> <span data-ttu-id="c059f-127">Skript může spuštěna v hello hlavního uzlu, hello pracovní uzly nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="c059f-127">A script can be ran on hello head node, hello worker nodes, or both.</span></span>

<span data-ttu-id="c059f-128">HDInsight nabízí několik hello tooinstall skripty v clusterech HDInsight následující součásti:</span><span class="sxs-lookup"><span data-stu-id="c059f-128">HDInsight provides several scripts tooinstall hello following components on HDInsight clusters:</span></span>

| <span data-ttu-id="c059f-129">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c059f-129">Name</span></span> | <span data-ttu-id="c059f-130">Skript</span><span class="sxs-lookup"><span data-stu-id="c059f-130">Script</span></span> |
| --- | --- |
| <span data-ttu-id="c059f-131">**Nainstalujte Spark**</span><span class="sxs-lookup"><span data-stu-id="c059f-131">**Install Spark**</span></span> |<span data-ttu-id="c059f-132">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="c059f-132">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="c059f-133">V tématu [instalací a použitím clustery Spark v HDInsight][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="c059f-133">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="c059f-134">**Nainstalujte jazyk R**</span><span class="sxs-lookup"><span data-stu-id="c059f-134">**Install R**</span></span> |<span data-ttu-id="c059f-135">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="c059f-135">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="c059f-136">V tématu [instalací a použitím R v clusterech HDInsight][hdinsight-install-r].</span><span class="sxs-lookup"><span data-stu-id="c059f-136">See [Install and use R on HDInsight clusters][hdinsight-install-r].</span></span> |
| <span data-ttu-id="c059f-137">**Nainstalujte Solr**</span><span class="sxs-lookup"><span data-stu-id="c059f-137">**Install Solr**</span></span> |<span data-ttu-id="c059f-138">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="c059f-138">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="c059f-139">V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="c059f-139">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="c059f-140">- **Nainstalujte Giraph**</span><span class="sxs-lookup"><span data-stu-id="c059f-140">- **Install Giraph**</span></span> |<span data-ttu-id="c059f-141">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="c059f-141">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="c059f-142">V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="c059f-142">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |
| <span data-ttu-id="c059f-143">**Předběžné načtení knihovny Hive**</span><span class="sxs-lookup"><span data-stu-id="c059f-143">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="c059f-144">https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="c059f-144">https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1.</span></span> <span data-ttu-id="c059f-145">V tématu [přidat Hive knihovny v clusterech prostředí HDInsight](hdinsight-hadoop-add-hive-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="c059f-145">See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md)</span></span> |

## <a name="call-scripts-using-hello-azure-portal"></a><span data-ttu-id="c059f-146">Volání skriptů pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c059f-146">Call scripts using hello Azure portal</span></span>
<span data-ttu-id="c059f-147">**Z hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="c059f-147">**From hello Azure portal**</span></span>

1. <span data-ttu-id="c059f-148">Zahájení vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="c059f-148">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
2. <span data-ttu-id="c059f-149">V části volitelné konfigurace pro hello **akcí skriptů** okně klikněte na tlačítko **přidat akce skriptu** tooprovide podrobností o hello akce skriptu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="c059f-149">Under Optional Configuration, for hello **Script Actions** blade, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="c059f-150">![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "použití akce skriptu toocustomize clusteru")</span><span class="sxs-lookup"><span data-stu-id="c059f-150">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="c059f-151">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c059f-151">Property</span></span></th><th><span data-ttu-id="c059f-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c059f-152">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="c059f-153">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c059f-153">Name</span></span></td>
            <td><span data-ttu-id="c059f-154">Zadejte název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-154">Specify a name for hello script action.</span></span></td></tr>
        <tr><td><span data-ttu-id="c059f-155">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="c059f-155">Script URI</span></span></td>
            <td><span data-ttu-id="c059f-156">Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c059f-156">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="c059f-157">s</span><span class="sxs-lookup"><span data-stu-id="c059f-157">s</span></span></td></tr>
        <tr><td><span data-ttu-id="c059f-158">HEAD/pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c059f-158">Head/Worker</span></span></td>
            <td><span data-ttu-id="c059f-159">Zadejte hello uzly (**Head** nebo **pracovní**) na úpravy hello je skript spuštěn.</b>.</span><span class="sxs-lookup"><span data-stu-id="c059f-159">Specify hello nodes (**Head** or **Worker**) on which hello customization script is run.</b>.</span></span>
        <tr><td><span data-ttu-id="c059f-160">Parametry</span><span class="sxs-lookup"><span data-stu-id="c059f-160">Parameters</span></span></td>
            <td><span data-ttu-id="c059f-161">Zadejte parametry hello, pokud to vyžaduje hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="c059f-161">Specify hello parameters, if required by hello script.</span></span></td></tr>
    </table>

    <span data-ttu-id="c059f-162">Stiskněte klávesu ENTER tooadd více než jeden skript akce tooinstall více součástí, které v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-162">Press ENTER tooadd more than one script action tooinstall multiple components on hello cluster.</span></span>
3. <span data-ttu-id="c059f-163">Klikněte na tlačítko **vyberte** toosave hello konfiguraci akce skriptu a pokračujte vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c059f-163">Click **Select** toosave hello script action configuration and continue with cluster creation.</span></span>

## <a name="call-scripts-using-azure-powershell"></a><span data-ttu-id="c059f-164">Volání skripty pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c059f-164">Call scripts using Azure PowerShell</span></span>
<span data-ttu-id="c059f-165">Tento následující skript prostředí PowerShell ukazuje, jak tooinstall Spark v systému Windows na základě clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c059f-165">This following PowerShell script demonstrates how tooinstall Spark on Windows based HDInsight cluster.</span></span>  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" toolist IDs.

    $nameToken = "<Enter A Name Token>"  # hello token is use toocreate Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect tooAzure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare hello dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify hello configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action toohello cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


<span data-ttu-id="c059f-166">tooinstall další software, budete potřebovat soubor skriptu hello tooreplace ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="c059f-166">tooinstall other software, you will need tooreplace hello script file in hello script:</span></span>

<span data-ttu-id="c059f-167">Po zobrazení výzvy zadejte přihlašovací údaje hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c059f-167">When prompted, enter hello credentials for hello cluster.</span></span> <span data-ttu-id="c059f-168">Může trvat několik minut, než se vytvoří hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c059f-168">It can take several minutes before hello cluster is created.</span></span>

## <a name="call-scripts-using-net-sdk"></a><span data-ttu-id="c059f-169">Volání skripty pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c059f-169">Call scripts using .NET SDK</span></span>
<span data-ttu-id="c059f-170">Hello následující příklad ukazuje, jak tooinstall Spark v systému Windows na základě clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c059f-170">hello following sample demonstrates how tooinstall Spark on Windows based HDInsight cluster.</span></span> <span data-ttu-id="c059f-171">tooinstall další software, budete potřebovat soubor skriptu hello tooreplace v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-171">tooinstall other software, you will need tooreplace hello script file in hello code.</span></span>

<span data-ttu-id="c059f-172">**toocreate clusteru HDInsight pomocí Spark**</span><span class="sxs-lookup"><span data-stu-id="c059f-172">**toocreate an HDInsight cluster with Spark**</span></span>

1. <span data-ttu-id="c059f-173">Vytvořte konzolovou aplikaci C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c059f-173">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="c059f-174">Z konzoly Správce balíčků Nuget hello spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-174">From hello Nuget Package Manager Console, run hello following command.</span></span>

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. <span data-ttu-id="c059f-175">Hello použijte následující příkazy using do souboru Program.cs hello:</span><span class="sxs-lookup"><span data-stu-id="c059f-175">Use hello following using statements in hello Program.cs file:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. <span data-ttu-id="c059f-176">Umístěte hello kód ve třídě hello s hello následující:</span><span class="sxs-lookup"><span data-stu-id="c059f-176">Place hello code in hello class with hello following:</span></span>

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is hello GUID for hello PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate tooan Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">hello AAD tenant ID</param>
        /// <param name="ClientId">hello AAD client ID</param>
        /// <param name="SubscriptionId">hello Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/",
                ClientId,
                new Uri("urn:ietf:wg:oauth:2.0:oob"),
                PromptBehavior.Always,
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for hello Resource manager and set hello subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register hello HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. <span data-ttu-id="c059f-177">Stiskněte klávesu **F5** toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c059f-177">Press **F5** toorun hello application.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="c059f-178">Podpora pro open-source softwaru použít na clustery HDInsight</span><span class="sxs-lookup"><span data-stu-id="c059f-178">Support for open-source software used on HDInsight clusters</span></span>
<span data-ttu-id="c059f-179">Hello služby Microsoft Azure HDInsight je flexibilní platforma, která umožňuje aplikacím toobuild velkých objemů dat v cloudu hello pomocí prostředí technologie open source vytvořen kolem Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c059f-179">hello Microsoft Azure HDInsight service is a flexible platform that enables you toobuild big-data applications in hello cloud by using an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="c059f-180">Microsoft Azure poskytuje určitou úroveň podpory pro technologie open source, jak je popsáno v hello **podporu oboru** části hello <a href="http://azure.microsoft.com/support/faq/" target="_blank">web Azure podporují – nejčastější dotazy</a>.</span><span class="sxs-lookup"><span data-stu-id="c059f-180">Microsoft Azure provides a general level of support for open-source technologies, as discussed in hello **Support Scope** section of hello <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ website</a>.</span></span> <span data-ttu-id="c059f-181">Hello služba HDInsight poskytuje další úroveň podpory pro některé z hello součástí, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="c059f-181">hello HDInsight service provides an additional level of support for some of hello components, as described below.</span></span>

<span data-ttu-id="c059f-182">Existují dva typy open-source komponent, které jsou k dispozici v hello služby HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c059f-182">There are two types of open-source components that are available in hello HDInsight service:</span></span>

* <span data-ttu-id="c059f-183">**Integrované komponenty** -tyto komponenty jsou předinstalované na clustery HDInsight a poskytují základní funkce služby hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c059f-183">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of hello cluster.</span></span> <span data-ttu-id="c059f-184">Například YARN ResourceManager, hello Hive dotazovací jazyk (HiveQL) a knihovna Mahout hello patří toothis kategorie.</span><span class="sxs-lookup"><span data-stu-id="c059f-184">For example, YARN ResourceManager, hello Hive query language (HiveQL), and hello Mahout library belong toothis category.</span></span> <span data-ttu-id="c059f-185">Úplný seznam součástí clusteru je k dispozici v [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight?](hdinsight-component-versioning.md) </a>.</span><span class="sxs-lookup"><span data-stu-id="c059f-185">A full list of cluster components is available in [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md)</a>.</span></span>
* <span data-ttu-id="c059f-186">**Vlastní komponenty** -, jako uživatel hello clusteru, můžete nainstalovat nebo použít v vaše úlohy žádné součásti k dispozici v komunitě hello nebo vytvořené vámi.</span><span class="sxs-lookup"><span data-stu-id="c059f-186">**Custom components** - You, as a user of hello cluster, can install or use in your workload any component available in hello community or created by you.</span></span>

<span data-ttu-id="c059f-187">Integrované komponenty jsou plně podporované, a bude pomoci tooisolate a vyřešit problémy související toothese součásti Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="c059f-187">Built-in components are fully supported, and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>

> [!WARNING]
> <span data-ttu-id="c059f-188">Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporované a Microsoft Support bude pomoci tooisolate a vyřešit problémy související toothese součásti.</span><span class="sxs-lookup"><span data-stu-id="c059f-188">Components provided with hello HDInsight cluster are fully supported and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="c059f-189">Vlastní komponenty přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-189">Custom components receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="c059f-190">To může způsobit řešení problému hello nebo žádat, že jste tooengage dostupné kanály pro hello otevřít zdroj technologie, kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="c059f-190">This might result in resolving hello issue OR asking you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="c059f-191">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c059f-191">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>
>
>

<span data-ttu-id="c059f-192">Hello služba HDInsight poskytuje několik způsobů toouse vlastní součásti.</span><span class="sxs-lookup"><span data-stu-id="c059f-192">hello HDInsight service provides several ways toouse custom components.</span></span> <span data-ttu-id="c059f-193">Bez ohledu na to, jak součást použít nebo nainstalovat na clusteru hello hello stejnou úroveň podpory platí.</span><span class="sxs-lookup"><span data-stu-id="c059f-193">Regardless of how a component is used or installed on hello cluster, hello same level of support applies.</span></span> <span data-ttu-id="c059f-194">Níže je seznam hello nejběžnější způsoby vlastní komponenty lze v clusterech HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c059f-194">Below is a list of hello most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="c059f-195">Úloha odeslání - Hadoop nebo jiné typy úloh, které spustit nebo používat vlastní komponenty může být odeslaná toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c059f-195">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted toohello cluster.</span></span>
2. <span data-ttu-id="c059f-196">Přizpůsobení clusteru – při vytváření clusteru, můžete zadat další nastavení a vlastní součásti, které se nainstalují na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-196">Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on hello cluster nodes.</span></span>
3. <span data-ttu-id="c059f-197">Ukázky - oblíbených vlastní součásti, Microsoft a ostatní mohou poskytnout ukázky použití těchto součástí v clusterech HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="c059f-197">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on hello HDInsight clusters.</span></span> <span data-ttu-id="c059f-198">Tyto soubory jsou uvedeny bez podpory.</span><span class="sxs-lookup"><span data-stu-id="c059f-198">These samples are provided without support.</span></span>

## <a name="develop-script-action-scripts"></a><span data-ttu-id="c059f-199">Vývoj skriptů akce skriptu</span><span class="sxs-lookup"><span data-stu-id="c059f-199">Develop Script Action scripts</span></span>
<span data-ttu-id="c059f-200">V tématu [vyvíjet akce skriptu skripty pro HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="c059f-200">See [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>

## <a name="see-also"></a><span data-ttu-id="c059f-201">Viz také</span><span class="sxs-lookup"><span data-stu-id="c059f-201">See also</span></span>
* <span data-ttu-id="c059f-202">[Vytvoření clusterů systému Hadoop v HDInsight] [ hdinsight-provision-cluster] obsahuje pokyny, jak clusteru toocreate HDInsight pomocí jiné možnosti vlastního nastavení.</span><span class="sxs-lookup"><span data-stu-id="c059f-202">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how toocreate an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="c059f-203">[Vývoj skriptů akce skriptu pro HDInsight][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="c059f-203">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="c059f-204">[Nainstalovat a používat Spark v HDInsight clustery][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="c059f-204">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="c059f-205">[Nainstalovat a používat R na clustery HDInsight][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="c059f-205">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="c059f-206">[Nainstalovat a používat Solr v clusterech HDInsight](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="c059f-206">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="c059f-207">[Nainstalovat a používat Giraph v clusterech HDInsight](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="c059f-207">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fáze při vytváření clusteru"
