---
title: "aaaManage Hadoop clusterů v HDInsight pomocí prostředí PowerShell – Azure | Microsoft Docs"
description: "Zjistěte, jak tooperform administrativní úlohy pro hello clusterů systému Hadoop v HDInsight pomocí Azure PowerShell."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 3df082d752fa8c703db82a54b82b740290af6729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a><span data-ttu-id="897ba-103">Správa clusterů systému Hadoop v HDInsight pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="897ba-103">Manage Hadoop clusters in HDInsight by using Azure PowerShell</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="897ba-104">Prostředí Azure PowerShell je výkonná skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správy vašich zatížení v Azure.</span><span class="sxs-lookup"><span data-stu-id="897ba-104">Azure PowerShell is a powerful scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="897ba-105">V tomto článku se dozvíte, jak používat clusterů systému Hadoop toomanage v Azure HDInsight pomocí místní konzoly Azure PowerShell prostřednictvím hello prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="897ba-105">In this article, you will learn how toomanage Hadoop clusters in Azure HDInsight by using a local Azure PowerShell console through hello use of Windows PowerShell.</span></span> <span data-ttu-id="897ba-106">Seznam hello hello rutiny prostředí HDInsight PowerShell, naleznete v části [reference k rutině HDInsight][hdinsight-powershell-reference].</span><span class="sxs-lookup"><span data-stu-id="897ba-106">For hello list of hello HDInsight PowerShell cmdlets, see [HDInsight cmdlet reference][hdinsight-powershell-reference].</span></span>

<span data-ttu-id="897ba-107">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="897ba-107">**Prerequisites**</span></span>

<span data-ttu-id="897ba-108">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-108">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="897ba-109">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="897ba-109">**An Azure subscription**.</span></span> <span data-ttu-id="897ba-110">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="897ba-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="897ba-111">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="897ba-111">Install Azure PowerShell</span></span>
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="897ba-112">Pokud jste nainstalovali Azure PowerShell verze 0,9 x, odinstalujte jej před instalací na novější verzi.</span><span class="sxs-lookup"><span data-stu-id="897ba-112">If you have installed Azure PowerShell version 0.9x, you must uninstall it before installing a newer version.</span></span>

<span data-ttu-id="897ba-113">nainstalovaná verze hello toocheck hello prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="897ba-113">toocheck hello version of hello installed PowerShell:</span></span>

    Get-Module *azure*

<span data-ttu-id="897ba-114">toouninstall hello starší verze, spouštět programy a funkce v Ovládacích panelech hello.</span><span class="sxs-lookup"><span data-stu-id="897ba-114">toouninstall hello older version, run Programs and Features in hello control panel.</span></span>

## <a name="create-clusters"></a><span data-ttu-id="897ba-115">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="897ba-115">Create clusters</span></span>
<span data-ttu-id="897ba-116">V tématu [vytvořit systémem Linux clusterů v HDInsight pomocí Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="897ba-116">See [Create Linux-based clusters in HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="897ba-117">Seznam clustery</span><span class="sxs-lookup"><span data-stu-id="897ba-117">List clusters</span></span>
<span data-ttu-id="897ba-118">Použijte následující příkaz toolist hello všechny clustery v aktuálním předplatném hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-118">Use hello following command toolist all clusters in hello current subscription:</span></span>

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a><span data-ttu-id="897ba-119">Zobrazit clusteru</span><span class="sxs-lookup"><span data-stu-id="897ba-119">Show cluster</span></span>
<span data-ttu-id="897ba-120">Použijte následující příkaz tooshow podrobnosti o konkrétní cluster v aktuálním předplatném hello hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-120">Use hello following command tooshow details of a specific cluster in hello current subscription:</span></span>

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a><span data-ttu-id="897ba-121">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="897ba-121">Delete clusters</span></span>
<span data-ttu-id="897ba-122">Použijte následující příkaz toodelete cluster hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-122">Use hello following command toodelete a cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

<span data-ttu-id="897ba-123">Můžete také odstranit cluster odebráním hello skupinu prostředků, který obsahuje hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="897ba-123">You can also delete a cluster by removing hello resource group that contains hello cluster.</span></span> <span data-ttu-id="897ba-124">Poznámka: Tato akce odstraní všechny hello prostředky ve skupině hello včetně hello výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="897ba-124">Please note, this will delete all hello resources in hello group including hello default storage account.</span></span>

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="897ba-125">Škálování clusterů</span><span class="sxs-lookup"><span data-stu-id="897ba-125">Scale clusters</span></span>
<span data-ttu-id="897ba-126">Hello clusteru škálování funkce vám umožní toochange hello počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti toore – vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="897ba-126">hello cluster scaling feature allows you toochange hello number of worker nodes used by a cluster that is running in Azure HDInsight without having toore-create hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="897ba-127">Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="897ba-127">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="897ba-128">Pokud si nejste jistí hello verze vašeho clusteru, můžete zkontrolovat hello stránku vlastností.</span><span class="sxs-lookup"><span data-stu-id="897ba-128">If you are unsure of hello version of your cluster, you can check hello Properties page.</span></span>  <span data-ttu-id="897ba-129">V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="897ba-129">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="897ba-130">Hello dopad změny hello počet uzlů dat pro každý typ clusteru podporuje HDInsight:</span><span class="sxs-lookup"><span data-stu-id="897ba-130">hello impact of changing hello number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="897ba-131">Hadoop</span><span class="sxs-lookup"><span data-stu-id="897ba-131">Hadoop</span></span>

    <span data-ttu-id="897ba-132">Můžete zvýšit bezproblémově hello počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné.</span><span class="sxs-lookup"><span data-stu-id="897ba-132">You can seamlessly increase hello number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="897ba-133">Nové úlohy můžete také odeslány, když probíhá operace hello.</span><span class="sxs-lookup"><span data-stu-id="897ba-133">New jobs can also be submitted while hello operation is in progress.</span></span> <span data-ttu-id="897ba-134">Selhání v rámci operace škálování pohodlné zpracování tak, aby hello clusteru vždy zůstává ve funkčním stavu.</span><span class="sxs-lookup"><span data-stu-id="897ba-134">Failures in a scaling operation are gracefully handled so that hello cluster is always left in a functional state.</span></span>

    <span data-ttu-id="897ba-135">Pokud se Hadoop cluster měřítko snížením hello počet uzlů data, některé služby hello v clusteru hello restartovat.</span><span class="sxs-lookup"><span data-stu-id="897ba-135">When a Hadoop cluster is scaled down by reducing hello number of data nodes, some of hello services in hello cluster are restarted.</span></span> <span data-ttu-id="897ba-136">To způsobí, že všechny spuštěné a čeká se na úlohy toofail na dokončení hello hello operace škálování.</span><span class="sxs-lookup"><span data-stu-id="897ba-136">This causes all running and pending jobs toofail at hello completion of hello scaling operation.</span></span> <span data-ttu-id="897ba-137">Po dokončení operace hello může, ale odešlete znovu hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="897ba-137">You can, however, resubmit hello jobs once hello operation is complete.</span></span>
* <span data-ttu-id="897ba-138">HBase</span><span class="sxs-lookup"><span data-stu-id="897ba-138">HBase</span></span>

    <span data-ttu-id="897ba-139">Můžete bezproblémově přidat nebo odebrat uzly clusteru HBase tooyour, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="897ba-139">You can seamlessly add or remove nodes tooyour HBase cluster while it is running.</span></span> <span data-ttu-id="897ba-140">Místní servery jsou automaticky vyváženy během několika minut od hello operace škálování.</span><span class="sxs-lookup"><span data-stu-id="897ba-140">Regional Servers are automatically balanced within a few minutes of completing hello scaling operation.</span></span> <span data-ttu-id="897ba-141">Ale můžete také ručně vyvážit hello místní servery přihlášením toohello headnode clusteru a spuštěné hello z okna příkazového řádku následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="897ba-141">However, you can also manually balance hello regional servers by logging in toohello headnode of cluster and running hello following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="897ba-142">Storm</span><span class="sxs-lookup"><span data-stu-id="897ba-142">Storm</span></span>

    <span data-ttu-id="897ba-143">Můžete bez problémů přidat nebo odebrat cluster Storm tooyour uzly dat je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="897ba-143">You can seamlessly add or remove data nodes tooyour Storm cluster while it is running.</span></span> <span data-ttu-id="897ba-144">Ale po úspěšném dokončení hello operace škálování, budete potřebovat toorebalance hello topologie.</span><span class="sxs-lookup"><span data-stu-id="897ba-144">But after a successful completion of hello scaling operation, you will need toorebalance hello topology.</span></span>

    <span data-ttu-id="897ba-145">Vyrovnává lze dosáhnout dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="897ba-145">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="897ba-146">Storm webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="897ba-146">Storm web UI</span></span>
  * <span data-ttu-id="897ba-147">Nástroj pro rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="897ba-147">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="897ba-148">Podrobnosti najdete toohello [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="897ba-148">Please refer toohello [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="897ba-149">Hello Storm webového uživatelského rozhraní není k dispozici v clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-149">hello Storm web UI is available on hello HDInsight cluster:</span></span>

    ![Obnovte rovnováhu škálování HDInsight storm](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    <span data-ttu-id="897ba-151">Tady je příklad jak toouse hello rozhraní příkazového řádku příkaz topologie Storm toorebalance hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-151">Here is an example how toouse hello CLI command toorebalance hello Storm topology:</span></span>

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="897ba-152">toochange hello velikost clusteru Hadoop pomocí prostředí Azure PowerShell, spusťte následující příkaz z klientský počítač hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-152">toochange hello Hadoop cluster size by using Azure PowerShell, run hello following command from a client machine:</span></span>

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a><span data-ttu-id="897ba-153">Udělení nebo odvolání přístupu</span><span class="sxs-lookup"><span data-stu-id="897ba-153">Grant/revoke access</span></span>
<span data-ttu-id="897ba-154">Clustery HDInsight mít hello následující HTTP webové služby (všechny tyto služby mají RESTful koncových bodů):</span><span class="sxs-lookup"><span data-stu-id="897ba-154">HDInsight clusters have hello following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="897ba-155">ODBC</span><span class="sxs-lookup"><span data-stu-id="897ba-155">ODBC</span></span>
* <span data-ttu-id="897ba-156">JDBC</span><span class="sxs-lookup"><span data-stu-id="897ba-156">JDBC</span></span>
* <span data-ttu-id="897ba-157">Ambari</span><span class="sxs-lookup"><span data-stu-id="897ba-157">Ambari</span></span>
* <span data-ttu-id="897ba-158">Oozie</span><span class="sxs-lookup"><span data-stu-id="897ba-158">Oozie</span></span>
* <span data-ttu-id="897ba-159">Templeton</span><span class="sxs-lookup"><span data-stu-id="897ba-159">Templeton</span></span>

<span data-ttu-id="897ba-160">Ve výchozím nastavení jsou tyto služby oprávnění pro přístup.</span><span class="sxs-lookup"><span data-stu-id="897ba-160">By default, these services are granted for access.</span></span> <span data-ttu-id="897ba-161">Můžete můžete odvolat nebo udělit přístup hello.</span><span class="sxs-lookup"><span data-stu-id="897ba-161">You can revoke/grant hello access.</span></span> <span data-ttu-id="897ba-162">toorevoke:</span><span class="sxs-lookup"><span data-stu-id="897ba-162">toorevoke:</span></span>

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

<span data-ttu-id="897ba-163">toogrant:</span><span class="sxs-lookup"><span data-stu-id="897ba-163">toogrant:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter hello Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter hello HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> <span data-ttu-id="897ba-164">Pomocí udělení nebo odvolání přístupu hello, obnoví hello clusteru uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="897ba-164">By granting/revoking hello access, you will reset hello cluster user name and password.</span></span>
>
>

<span data-ttu-id="897ba-165">To lze provést také prostřednictvím hello portálu.</span><span class="sxs-lookup"><span data-stu-id="897ba-165">This can also be done via hello Portal.</span></span> <span data-ttu-id="897ba-166">V tématu [hello spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="897ba-166">See [Administer HDInsight by using hello Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="897ba-167">Aktualizovat pověření uživatele HTTP</span><span class="sxs-lookup"><span data-stu-id="897ba-167">Update HTTP user credentials</span></span>
<span data-ttu-id="897ba-168">Je hello stejný postup jako [HTTP udělení nebo odvolání přístupu](#grant/revoke-access). Pokud hello clusteru byla udělena hello přístup protokolu HTTP, musí se nejdřív odvolat.</span><span class="sxs-lookup"><span data-stu-id="897ba-168">It is hello same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If hello cluster has been granted hello HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="897ba-169">A pak udělují přístup hello s novými pověřeními uživatele HTTP.</span><span class="sxs-lookup"><span data-stu-id="897ba-169">And then grant hello access with new HTTP user credentials.</span></span>

## <a name="find-hello-default-storage-account"></a><span data-ttu-id="897ba-170">Najít hello výchozí účet úložiště</span><span class="sxs-lookup"><span data-stu-id="897ba-170">Find hello default storage account</span></span>
<span data-ttu-id="897ba-171">Hello následující skript prostředí Powershell ukazuje, jak tooget hello výchozí název účtu úložiště a hello výchozí klíč účtu úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="897ba-171">hello following Powershell script demonstrates how tooget hello default storage account name and hello default storage account key for a cluster.</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-hello-resource-group"></a><span data-ttu-id="897ba-172">Najít skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="897ba-172">Find hello resource group</span></span>
<span data-ttu-id="897ba-173">V režimu Resource Manager hello patří každý cluster HDInsight tooan skupina prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="897ba-173">In hello Resource Manager mode, each HDInsight cluster belongs tooan Azure resource group.</span></span>  <span data-ttu-id="897ba-174">Skupina prostředků toofind hello:</span><span class="sxs-lookup"><span data-stu-id="897ba-174">toofind hello resource group:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a><span data-ttu-id="897ba-175">Odesílání úloh</span><span class="sxs-lookup"><span data-stu-id="897ba-175">Submit jobs</span></span>
<span data-ttu-id="897ba-176">**úlohy MapReduce toosubmit**</span><span class="sxs-lookup"><span data-stu-id="897ba-176">**toosubmit MapReduce jobs**</span></span>

<span data-ttu-id="897ba-177">V tématu [ukázky spustit Hadoop MapReduce v HDInsight se systémem Windows](hdinsight-run-samples.md).</span><span class="sxs-lookup"><span data-stu-id="897ba-177">See [Run Hadoop MapReduce samples in Windows-based HDInsight](hdinsight-run-samples.md).</span></span>

<span data-ttu-id="897ba-178">**toosubmit úloh Hive**</span><span class="sxs-lookup"><span data-stu-id="897ba-178">**toosubmit Hive jobs**</span></span>

<span data-ttu-id="897ba-179">V tématu [spouštění dotazů Hive pomocí prostředí PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="897ba-179">See [Run Hive queries using PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span></span>

<span data-ttu-id="897ba-180">**úlohy Pig toosubmit**</span><span class="sxs-lookup"><span data-stu-id="897ba-180">**toosubmit Pig jobs**</span></span>

<span data-ttu-id="897ba-181">V tématu [úlohy Pig spuštění pomocí prostředí PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="897ba-181">See [Run Pig jobs using PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span></span>

<span data-ttu-id="897ba-182">**toosubmit Sqoop úlohy**</span><span class="sxs-lookup"><span data-stu-id="897ba-182">**toosubmit Sqoop jobs**</span></span>

<span data-ttu-id="897ba-183">V tématu [použití nástroje Sqoop se HDInsight](hdinsight-use-sqoop.md).</span><span class="sxs-lookup"><span data-stu-id="897ba-183">See [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).</span></span>

<span data-ttu-id="897ba-184">**toosubmit Oozie úlohy**</span><span class="sxs-lookup"><span data-stu-id="897ba-184">**toosubmit Oozie jobs**</span></span>

<span data-ttu-id="897ba-185">V tématu [Oozie použití s Hadoop toodefine a spouštění pracovních postupů v prostředí HDInsight](hdinsight-use-oozie.md).</span><span class="sxs-lookup"><span data-stu-id="897ba-185">See [Use Oozie with Hadoop toodefine and run a workflow in HDInsight](hdinsight-use-oozie.md).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="897ba-186">Nahrání dat tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="897ba-186">Upload data tooAzure Blob storage</span></span>
<span data-ttu-id="897ba-187">V tématu [nahrát data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="897ba-187">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="897ba-188">Viz také</span><span class="sxs-lookup"><span data-stu-id="897ba-188">See Also</span></span>
* <span data-ttu-id="897ba-189">[HDInsight rutiny referenční dokumentaci k nástroji][hdinsight-powershell-reference]</span><span class="sxs-lookup"><span data-stu-id="897ba-189">[HDInsight cmdlet reference documentation][hdinsight-powershell-reference]</span></span>
* <span data-ttu-id="897ba-190">[Spravovat HDInsight pomocí hello portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="897ba-190">[Administer HDInsight by using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="897ba-191">[Spravovat HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="897ba-191">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="897ba-192">[Vytvoření clusterů HDInsight][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="897ba-192">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="897ba-193">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="897ba-193">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="897ba-194">[Odesílání úloh Hadoop prostřednictvím kódu programu][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="897ba-194">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="897ba-195">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="897ba-195">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
