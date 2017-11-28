---
title: "Přizpůsobení clusterů HDInsight pomocí akcí skriptů - Azure | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit clusterů HDInsight pomocí akce skriptu."
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
ms.openlocfilehash: ec95b6d66c71b4278dd1e16807fcc75f5e8b1c36
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="9421a-103">Přizpůsobení clusterů HDInsight se systémem Windows pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="9421a-103">Customize Windows-based HDInsight clusters using Script Action</span></span>
<span data-ttu-id="9421a-104">**Skript akce** slouží k vyvolání [vlastní skripty](hdinsight-hadoop-script-actions.md) během procesu vytváření clusteru pro instalaci další software v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-104">**Script Action** can be used to invoke [custom scripts](hdinsight-hadoop-script-actions.md) during the cluster creation process for installing additional software on a cluster.</span></span>

<span data-ttu-id="9421a-105">Informace v tomto článku jsou specifické pro clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="9421a-105">The information in this article is specific to Windows-based HDInsight clusters.</span></span> <span data-ttu-id="9421a-106">V clusterech se systémem Linux naleznete v části [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="9421a-106">For Linux-based clusters, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9421a-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="9421a-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="9421a-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="9421a-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="9421a-109">Clustery prostředí HDInsight lze přizpůsobit v různých další způsoby, například včetně další účty Azure Storage, změna Hadoop soubory konfigurace (core-site.xml, hive-site.xml atd.) nebo přidání sdílené knihovny (například Hive, Oozie) do společné umístění v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-109">HDInsight clusters can be customized in a variety of other ways as well, such as including additional Azure Storage accounts, changing the Hadoop configuration files (core-site.xml, hive-site.xml, etc.), or adding shared libraries (e.g., Hive, Oozie) into common locations in the cluster.</span></span> <span data-ttu-id="9421a-110">Toto vlastní nastavení můžete provést pomocí prostředí Azure PowerShell, .NET SDK služby Azure HDInsight nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9421a-110">These customizations can be done through Azure PowerShell, the Azure HDInsight .NET SDK, or the Azure portal.</span></span> <span data-ttu-id="9421a-111">Další informace najdete v tématu [vytvoření Hadoop clusterů v HDInsight][hdinsight-provision-cluster].</span><span class="sxs-lookup"><span data-stu-id="9421a-111">For more information, see [Create Hadoop clusters in HDInsight][hdinsight-provision-cluster].</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a><span data-ttu-id="9421a-112">Akce skriptu v procesu vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="9421a-112">Script Action in the cluster creation process</span></span>
<span data-ttu-id="9421a-113">Akce skriptu se používá pouze při clusteru je právě vytvářena.</span><span class="sxs-lookup"><span data-stu-id="9421a-113">Script Action is only used while a cluster is in the process of being created.</span></span> <span data-ttu-id="9421a-114">Následující diagram znázorňuje, když během procesu vytváření provedení akce skriptu:</span><span class="sxs-lookup"><span data-stu-id="9421a-114">The following diagram illustrates when Script Action is executed during the creation process:</span></span>

<span data-ttu-id="9421a-115">![Přizpůsobení cluster HDInsight a fáze při vytváření clusteru][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="9421a-115">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="9421a-116">Když je skript spuštěn, cluster zadá **ClusterCustomization** fáze.</span><span class="sxs-lookup"><span data-stu-id="9421a-116">When the script is running, the cluster enters the **ClusterCustomization** stage.</span></span> <span data-ttu-id="9421a-117">V této fázi skript běží pod účtem správce systému, paralelně na všechny zadané uzly v clusteru a poskytuje úplný správce oprávnění na uzlech.</span><span class="sxs-lookup"><span data-stu-id="9421a-117">At this stage, the script is run under the system admin account, in parallel on all the specified nodes in the cluster, and provides full admin privileges on the nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="9421a-118">Vzhledem k tomu, že máte oprávnění správce na uzlech clusteru během **ClusterCustomization** fáze, můžete použít skript k provedení operací, jako je spouštění a zastavování služeb, včetně služby související s Hadoop.</span><span class="sxs-lookup"><span data-stu-id="9421a-118">Because you have admin privileges on the cluster nodes during the **ClusterCustomization** stage, you can use the script to perform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="9421a-119">Ano, v rámci skriptu je nutné zajistit, že služby Ambari a další služby související s Hadoop jsou spuštěny před dokončení spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="9421a-119">So, as part of the script, you must ensure that the Ambari services and other Hadoop-related services are up and running before the script finishes running.</span></span> <span data-ttu-id="9421a-120">Tyto služby jsou nezbytné pro úspěšně zjištění stavu a stavu clusteru při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="9421a-120">These services are required to successfully ascertain the health and state of the cluster while it is being created.</span></span> <span data-ttu-id="9421a-121">Pokud změníte žádnou konfiguraci v clusteru, který má vliv na tyto služby, musíte použít pomocných funkcí, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9421a-121">If you change any configuration on the cluster that affects these services, you must use the helper functions that are provided.</span></span> <span data-ttu-id="9421a-122">Další informace o pomocných funkcí najdete v tématu [vyvíjet akce skriptu skripty pro HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="9421a-122">For more information about helper functions, see [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>
>
>

<span data-ttu-id="9421a-123">Výstup a protokoly chyb pro skript ukládají na výchozí účet úložiště, které jste zadali pro cluster.</span><span class="sxs-lookup"><span data-stu-id="9421a-123">The output and the error logs for the script are stored in the default Storage account you specified for the cluster.</span></span> <span data-ttu-id="9421a-124">Protokoly jsou uložené v tabulce s názvem **u < \cluster-name-fragment >< \time-stamp > setuplog**.</span><span class="sxs-lookup"><span data-stu-id="9421a-124">The logs are stored in a table with the name **u<\cluster-name-fragment><\time-stamp>setuplog**.</span></span> <span data-ttu-id="9421a-125">Toto jsou agregovaná protokoly ze skriptu, spusťte na všech uzlech (hlavního uzlu a pracovní uzly) v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-125">These are aggregate logs from the script run on all the nodes (head node and worker nodes) in the cluster.</span></span>

<span data-ttu-id="9421a-126">Každý cluster může přijmout víc akcí skriptů, které je vyvolán v pořadí, ve kterém jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="9421a-126">Each cluster can accept multiple script actions that are invoked in the order in which they are specified.</span></span> <span data-ttu-id="9421a-127">Skript může spuštěna v hlavního uzlu, uzlů pracovního procesu nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="9421a-127">A script can be ran on the head node, the worker nodes, or both.</span></span>

<span data-ttu-id="9421a-128">HDInsight nabízí několik skriptů v clusterech HDInsight nainstalovat následující součásti:</span><span class="sxs-lookup"><span data-stu-id="9421a-128">HDInsight provides several scripts to install the following components on HDInsight clusters:</span></span>

| <span data-ttu-id="9421a-129">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9421a-129">Name</span></span> | <span data-ttu-id="9421a-130">Skript</span><span class="sxs-lookup"><span data-stu-id="9421a-130">Script</span></span> |
| --- | --- |
| <span data-ttu-id="9421a-131">**Nainstalujte Spark**</span><span class="sxs-lookup"><span data-stu-id="9421a-131">**Install Spark**</span></span> |<span data-ttu-id="9421a-132">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="9421a-132">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="9421a-133">V tématu [instalací a použitím clustery Spark v HDInsight][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="9421a-133">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="9421a-134">**Nainstalujte jazyk R**</span><span class="sxs-lookup"><span data-stu-id="9421a-134">**Install R**</span></span> |<span data-ttu-id="9421a-135">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="9421a-135">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="9421a-136">V tématu [instalací a použitím R v clusterech HDInsight][hdinsight-install-r].</span><span class="sxs-lookup"><span data-stu-id="9421a-136">See [Install and use R on HDInsight clusters][hdinsight-install-r].</span></span> |
| <span data-ttu-id="9421a-137">**Nainstalujte Solr**</span><span class="sxs-lookup"><span data-stu-id="9421a-137">**Install Solr**</span></span> |<span data-ttu-id="9421a-138">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="9421a-138">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="9421a-139">V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="9421a-139">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="9421a-140">- **Nainstalujte Giraph**</span><span class="sxs-lookup"><span data-stu-id="9421a-140">- **Install Giraph**</span></span> |<span data-ttu-id="9421a-141">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="9421a-141">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="9421a-142">V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="9421a-142">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |
| <span data-ttu-id="9421a-143">**Předběžné načtení knihovny Hive**</span><span class="sxs-lookup"><span data-stu-id="9421a-143">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="9421a-144">https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="9421a-144">https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1.</span></span> <span data-ttu-id="9421a-145">V tématu [přidat Hive knihovny v clusterech prostředí HDInsight](hdinsight-hadoop-add-hive-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="9421a-145">See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md)</span></span> |

## <a name="call-scripts-using-the-azure-portal"></a><span data-ttu-id="9421a-146">Volání skripty pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9421a-146">Call scripts using the Azure portal</span></span>
<span data-ttu-id="9421a-147">**Z portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="9421a-147">**From the Azure portal**</span></span>

1. <span data-ttu-id="9421a-148">Zahájení vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="9421a-148">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
2. <span data-ttu-id="9421a-149">V části volitelné konfigurace pro **akcí skriptů** okně klikněte na tlačítko **přidat akce skriptu** poskytnout podrobnosti o akce skriptu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="9421a-149">Under Optional Configuration, for the **Script Actions** blade, click **add script action** to provide details about the script action, as shown below:</span></span>

    <span data-ttu-id="9421a-150">![Použití akce skriptu k přizpůsobení cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "použití akce skriptu k přizpůsobení clusteru")</span><span class="sxs-lookup"><span data-stu-id="9421a-150">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="9421a-151">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9421a-151">Property</span></span></th><th><span data-ttu-id="9421a-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="9421a-152">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="9421a-153">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9421a-153">Name</span></span></td>
            <td><span data-ttu-id="9421a-154">Zadejte název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9421a-154">Specify a name for the script action.</span></span></td></tr>
        <tr><td><span data-ttu-id="9421a-155">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="9421a-155">Script URI</span></span></td>
            <td><span data-ttu-id="9421a-156">Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-156">Specify the URI to the script that is invoked to customize the cluster.</span></span> <span data-ttu-id="9421a-157">s</span><span class="sxs-lookup"><span data-stu-id="9421a-157">s</span></span></td></tr>
        <tr><td><span data-ttu-id="9421a-158">HEAD/pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="9421a-158">Head/Worker</span></span></td>
            <td><span data-ttu-id="9421a-159">Zadejte uzly (**Head** nebo **pracovní**) podle kterého se spouští skript přizpůsobení.</b>.</span><span class="sxs-lookup"><span data-stu-id="9421a-159">Specify the nodes (**Head** or **Worker**) on which the customization script is run.</b>.</span></span>
        <tr><td><span data-ttu-id="9421a-160">Parametry</span><span class="sxs-lookup"><span data-stu-id="9421a-160">Parameters</span></span></td>
            <td><span data-ttu-id="9421a-161">Zadejte parametry, pokud se vyžadují skriptem.</span><span class="sxs-lookup"><span data-stu-id="9421a-161">Specify the parameters, if required by the script.</span></span></td></tr>
    </table>

    <span data-ttu-id="9421a-162">Stiskněte klávesu ENTER, chcete-li přidat více než jednu akci skriptu pro instalaci více součástí v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-162">Press ENTER to add more than one script action to install multiple components on the cluster.</span></span>
3. <span data-ttu-id="9421a-163">Klikněte na tlačítko **vyberte** uložit konfiguraci akce skriptu a pokračujte vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-163">Click **Select** to save the script action configuration and continue with cluster creation.</span></span>

## <a name="call-scripts-using-azure-powershell"></a><span data-ttu-id="9421a-164">Volání skripty pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9421a-164">Call scripts using Azure PowerShell</span></span>
<span data-ttu-id="9421a-165">Tento následující skript prostředí PowerShell ukazuje, jak nainstalovat Spark na clusteru HDInsight založené na systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9421a-165">This following PowerShell script demonstrates how to install Spark on Windows based HDInsight cluster.</span></span>  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
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

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action to the cluster configuration
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


<span data-ttu-id="9421a-166">Pokud chcete nainstalovat další software, budete muset nahradit soubor skriptu ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="9421a-166">To install other software, you will need to replace the script file in the script:</span></span>

<span data-ttu-id="9421a-167">Po zobrazení výzvy zadejte přihlašovací údaje pro cluster.</span><span class="sxs-lookup"><span data-stu-id="9421a-167">When prompted, enter the credentials for the cluster.</span></span> <span data-ttu-id="9421a-168">To může trvat několik minut, než je vytvořen cluster.</span><span class="sxs-lookup"><span data-stu-id="9421a-168">It can take several minutes before the cluster is created.</span></span>

## <a name="call-scripts-using-net-sdk"></a><span data-ttu-id="9421a-169">Volání skripty pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9421a-169">Call scripts using .NET SDK</span></span>
<span data-ttu-id="9421a-170">Následující příklad ukazuje, jak nainstalovat Spark na clusteru HDInsight založené na systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9421a-170">The following sample demonstrates how to install Spark on Windows based HDInsight cluster.</span></span> <span data-ttu-id="9421a-171">Pokud chcete nainstalovat další software, potřebujete nahradit soubor skriptu v kódu.</span><span class="sxs-lookup"><span data-stu-id="9421a-171">To install other software, you will need to replace the script file in the code.</span></span>

<span data-ttu-id="9421a-172">**Vytvoření clusteru HDInsight pomocí Spark**</span><span class="sxs-lookup"><span data-stu-id="9421a-172">**To create an HDInsight cluster with Spark**</span></span>

1. <span data-ttu-id="9421a-173">Vytvořte konzolovou aplikaci C# v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9421a-173">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="9421a-174">Z konzoly Správce balíčků Nuget spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="9421a-174">From the Nuget Package Manager Console, run the following command.</span></span>

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. <span data-ttu-id="9421a-175">Použijte následující příkazy using do souboru Program.cs:</span><span class="sxs-lookup"><span data-stu-id="9421a-175">Use the following using statements in the Program.cs file:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. <span data-ttu-id="9421a-176">Umístěte kód ve třídě, s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="9421a-176">Place the code in the class with the following:</span></span>

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
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
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
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
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. <span data-ttu-id="9421a-177">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9421a-177">Press **F5** to run the application.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="9421a-178">Podpora pro open-source softwaru použít na clustery HDInsight</span><span class="sxs-lookup"><span data-stu-id="9421a-178">Support for open-source software used on HDInsight clusters</span></span>
<span data-ttu-id="9421a-179">Služba Microsoft Azure HDInsight je flexibilní platforma, která vám umožní sestavovat aplikace velkých objemů dat v cloudu pomocí prostředí technologie open source vytvořen kolem Hadoop.</span><span class="sxs-lookup"><span data-stu-id="9421a-179">The Microsoft Azure HDInsight service is a flexible platform that enables you to build big-data applications in the cloud by using an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="9421a-180">Microsoft Azure poskytuje určitou úroveň podpory pro technologie open source, jak je popsáno v **podporu oboru** části <a href="http://azure.microsoft.com/support/faq/" target="_blank">web Azure podporují – nejčastější dotazy</a>.</span><span class="sxs-lookup"><span data-stu-id="9421a-180">Microsoft Azure provides a general level of support for open-source technologies, as discussed in the **Support Scope** section of the <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ website</a>.</span></span> <span data-ttu-id="9421a-181">Služba HDInsight poskytuje další úroveň podpory pro některé součásti, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="9421a-181">The HDInsight service provides an additional level of support for some of the components, as described below.</span></span>

<span data-ttu-id="9421a-182">Existují dva typy open-source komponent, které jsou k dispozici ve službě HDInsight:</span><span class="sxs-lookup"><span data-stu-id="9421a-182">There are two types of open-source components that are available in the HDInsight service:</span></span>

* <span data-ttu-id="9421a-183">**Integrované komponenty** -tyto komponenty jsou předinstalované na clustery HDInsight a poskytují základní funkce služby clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-183">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of the cluster.</span></span> <span data-ttu-id="9421a-184">Například YARN ResourceManager, Hive dotazovací jazyk (HiveQL) a knihovně Mahout patří do této kategorie.</span><span class="sxs-lookup"><span data-stu-id="9421a-184">For example, YARN ResourceManager, the Hive query language (HiveQL), and the Mahout library belong to this category.</span></span> <span data-ttu-id="9421a-185">Úplný seznam součástí clusteru je k dispozici v [co je nového ve verzích clusterů systému Hadoop poskytovaných prostředím HDInsight?](hdinsight-component-versioning.md) </a>.</span><span class="sxs-lookup"><span data-stu-id="9421a-185">A full list of cluster components is available in [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md)</a>.</span></span>
* <span data-ttu-id="9421a-186">**Vlastní komponenty** -, jako uživatel clusteru, můžete nainstalovat nebo použít v vaše úlohy žádné součásti k dispozici v komunitě nebo vytvořené vámi.</span><span class="sxs-lookup"><span data-stu-id="9421a-186">**Custom components** - You, as a user of the cluster, can install or use in your workload any component available in the community or created by you.</span></span>

<span data-ttu-id="9421a-187">Integrované komponenty jsou plně podporované a Microsoft Support pomůže k izolování a vyřešení problémů týkajících se těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="9421a-187">Built-in components are fully supported, and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>

> [!WARNING]
> <span data-ttu-id="9421a-188">Součásti, které jsou součástí clusteru HDInsight jsou plně podporované a Microsoft Support pomůže k izolování a vyřešení problémů týkajících se těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="9421a-188">Components provided with the HDInsight cluster are fully supported and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>
>
> <span data-ttu-id="9421a-189">Vlastní komponenty získat vyvineme podporu k pomoci při další řešení problému.</span><span class="sxs-lookup"><span data-stu-id="9421a-189">Custom components receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="9421a-190">To může způsobit řešení problému nebo s žádostí o zapojení dostupné kanály pro technologie s otevřeným zdrojem, kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="9421a-190">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="9421a-191">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="9421a-191">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="9421a-192">Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="9421a-192">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>
>
>

<span data-ttu-id="9421a-193">Služba HDInsight poskytuje několik způsobů, jak používat vlastní komponenty.</span><span class="sxs-lookup"><span data-stu-id="9421a-193">The HDInsight service provides several ways to use custom components.</span></span> <span data-ttu-id="9421a-194">Bez ohledu na to, jak součást použít nebo nainstalované v clusteru se vztahuje stejnou úroveň podpory.</span><span class="sxs-lookup"><span data-stu-id="9421a-194">Regardless of how a component is used or installed on the cluster, the same level of support applies.</span></span> <span data-ttu-id="9421a-195">Níže je seznam nejběžnější způsoby vlastní komponenty lze v clusterech HDInsight:</span><span class="sxs-lookup"><span data-stu-id="9421a-195">Below is a list of the most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="9421a-196">Úloha odeslání - Hadoop nebo jiné typy úloh, které spustit nebo používat vlastní komponenty lze odeslat do clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-196">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted to the cluster.</span></span>
2. <span data-ttu-id="9421a-197">Přizpůsobení clusteru – při vytváření clusteru, můžete zadat další nastavení a vlastní součásti, které se nainstalují na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="9421a-197">Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on the cluster nodes.</span></span>
3. <span data-ttu-id="9421a-198">Ukázky - oblíbených vlastní součásti, Microsoft a ostatní mohou poskytnout ukázky použití těchto součástí v clusterech HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9421a-198">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on the HDInsight clusters.</span></span> <span data-ttu-id="9421a-199">Tyto soubory jsou uvedeny bez podpory.</span><span class="sxs-lookup"><span data-stu-id="9421a-199">These samples are provided without support.</span></span>

## <a name="develop-script-action-scripts"></a><span data-ttu-id="9421a-200">Vývoj skriptů akce skriptu</span><span class="sxs-lookup"><span data-stu-id="9421a-200">Develop Script Action scripts</span></span>
<span data-ttu-id="9421a-201">V tématu [vyvíjet akce skriptu skripty pro HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="9421a-201">See [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>

## <a name="see-also"></a><span data-ttu-id="9421a-202">Viz také</span><span class="sxs-lookup"><span data-stu-id="9421a-202">See also</span></span>
* <span data-ttu-id="9421a-203">[Vytvoření clusterů systému Hadoop v HDInsight] [ hdinsight-provision-cluster] obsahuje pokyny k vytvoření clusteru HDInsight pomocí jiné možnosti vlastního nastavení.</span><span class="sxs-lookup"><span data-stu-id="9421a-203">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how to create an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="9421a-204">[Vývoj skriptů akce skriptu pro HDInsight][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="9421a-204">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="9421a-205">[Nainstalovat a používat Spark v HDInsight clustery][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="9421a-205">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="9421a-206">[Nainstalovat a používat R na clustery HDInsight][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="9421a-206">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="9421a-207">[Nainstalovat a používat Solr v clusterech HDInsight](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="9421a-207">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="9421a-208">[Nainstalovat a používat Giraph v clusterech HDInsight](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="9421a-208">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


<span data-ttu-id="9421a-209">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fáze při vytváření clusteru"</span><span class="sxs-lookup"><span data-stu-id="9421a-209">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Stages during cluster creation"</span></span>
