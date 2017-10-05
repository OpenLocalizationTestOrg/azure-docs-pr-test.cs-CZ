---
title: "Správa clusterů systému Hadoop v HDInsight pomocí prostředí PowerShell – Azure | Microsoft Docs"
description: "Zjistěte, jak k provádění úloh správy pro clusterů systému Hadoop v HDInsight pomocí Azure PowerShell."
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
ms.openlocfilehash: c47dabd7c4aa4ba0be08c419989e536711f03677
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a><span data-ttu-id="bef50-103">Správa clusterů systému Hadoop v HDInsight pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bef50-103">Manage Hadoop clusters in HDInsight by using Azure PowerShell</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="bef50-104">Prostředí Azure PowerShell je výkonná skriptovací prostředí, které můžete řídit a automatizovat nasazení a správy vašich zatížení v Azure.</span><span class="sxs-lookup"><span data-stu-id="bef50-104">Azure PowerShell is a powerful scripting environment that you can use to control and automate the deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="bef50-105">V tomto článku se dozvíte, jak spravovat clustery systému Hadoop v prostředí Azure HDInsight pomocí místní konzoly Azure PowerShell pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bef50-105">In this article, you will learn how to manage Hadoop clusters in Azure HDInsight by using a local Azure PowerShell console through the use of Windows PowerShell.</span></span> <span data-ttu-id="bef50-106">Seznam rutin prostředí HDInsight PowerShell, naleznete v části [reference k rutině HDInsight][hdinsight-powershell-reference].</span><span class="sxs-lookup"><span data-stu-id="bef50-106">For the list of the HDInsight PowerShell cmdlets, see [HDInsight cmdlet reference][hdinsight-powershell-reference].</span></span>

<span data-ttu-id="bef50-107">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="bef50-107">**Prerequisites**</span></span>

<span data-ttu-id="bef50-108">Je nutné, abyste před zahájením tohoto článku měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="bef50-108">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="bef50-109">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="bef50-109">**An Azure subscription**.</span></span> <span data-ttu-id="bef50-110">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="bef50-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="bef50-111">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bef50-111">Install Azure PowerShell</span></span>
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="bef50-112">Pokud jste nainstalovali Azure PowerShell verze 0,9 x, odinstalujte jej před instalací na novější verzi.</span><span class="sxs-lookup"><span data-stu-id="bef50-112">If you have installed Azure PowerShell version 0.9x, you must uninstall it before installing a newer version.</span></span>

<span data-ttu-id="bef50-113">Kontrola verze nainstalované prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="bef50-113">To check the version of the installed PowerShell:</span></span>

    Get-Module *azure*

<span data-ttu-id="bef50-114">Chcete-li odinstalovat starší verzi, spusťte programy a funkce v Ovládacích panelech.</span><span class="sxs-lookup"><span data-stu-id="bef50-114">To uninstall the older version, run Programs and Features in the control panel.</span></span>

## <a name="create-clusters"></a><span data-ttu-id="bef50-115">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="bef50-115">Create clusters</span></span>
<span data-ttu-id="bef50-116">V tématu [vytvořit systémem Linux clusterů v HDInsight pomocí Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="bef50-116">See [Create Linux-based clusters in HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="bef50-117">Seznam clustery</span><span class="sxs-lookup"><span data-stu-id="bef50-117">List clusters</span></span>
<span data-ttu-id="bef50-118">Seznam všech clusterech v aktuálním předplatném použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bef50-118">Use the following command to list all clusters in the current subscription:</span></span>

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a><span data-ttu-id="bef50-119">Zobrazit clusteru</span><span class="sxs-lookup"><span data-stu-id="bef50-119">Show cluster</span></span>
<span data-ttu-id="bef50-120">Chcete-li zobrazit podrobnosti o konkrétní cluster v aktuálním předplatném použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bef50-120">Use the following command to show details of a specific cluster in the current subscription:</span></span>

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a><span data-ttu-id="bef50-121">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="bef50-121">Delete clusters</span></span>
<span data-ttu-id="bef50-122">Pokud chcete odstranit cluster použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bef50-122">Use the following command to delete a cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

<span data-ttu-id="bef50-123">Můžete také odstranit cluster odebráním skupinu prostředků, která obsahuje clusteru.</span><span class="sxs-lookup"><span data-stu-id="bef50-123">You can also delete a cluster by removing the resource group that contains the cluster.</span></span> <span data-ttu-id="bef50-124">Poznámka: Tato akce odstraní všechny prostředky ve skupině, včetně výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bef50-124">Please note, this will delete all the resources in the group including the default storage account.</span></span>

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="bef50-125">Škálování clusterů</span><span class="sxs-lookup"><span data-stu-id="bef50-125">Scale clusters</span></span>
<span data-ttu-id="bef50-126">Funkce škálování clusteru umožňuje změnit počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti znovu vytvořit cluster.</span><span class="sxs-lookup"><span data-stu-id="bef50-126">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="bef50-127">Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="bef50-127">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="bef50-128">Pokud si nejste jistí na verzi vašeho clusteru, můžete zkontrolovat stránku vlastností.</span><span class="sxs-lookup"><span data-stu-id="bef50-128">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="bef50-129">V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="bef50-129">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="bef50-130">Dopad změny v počtu uzlů dat pro každý typ clusteru podporuje HDInsight:</span><span class="sxs-lookup"><span data-stu-id="bef50-130">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="bef50-131">Hadoop</span><span class="sxs-lookup"><span data-stu-id="bef50-131">Hadoop</span></span>

    <span data-ttu-id="bef50-132">Můžete bez problémů zvýšit počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné.</span><span class="sxs-lookup"><span data-stu-id="bef50-132">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="bef50-133">Nové úlohy můžete také odeslány, když probíhá operace.</span><span class="sxs-lookup"><span data-stu-id="bef50-133">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="bef50-134">Selhání v rámci operace škálování pohodlné zpracování tak, aby cluster zůstane vždy ve funkčním stavu.</span><span class="sxs-lookup"><span data-stu-id="bef50-134">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>

    <span data-ttu-id="bef50-135">Pokud se Hadoop cluster měřítko snížením počtu uzlů data, některé služby v clusteru restartovat.</span><span class="sxs-lookup"><span data-stu-id="bef50-135">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="bef50-136">To způsobí, že všechny spuštěné a čeká se na úlohy selhání po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="bef50-136">This causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="bef50-137">Můžete, ale odešlete znovu úloh po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="bef50-137">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="bef50-138">HBase</span><span class="sxs-lookup"><span data-stu-id="bef50-138">HBase</span></span>

    <span data-ttu-id="bef50-139">Bezproblémově můžete přidávat nebo odebírat uzly do clusteru HBase, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="bef50-139">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="bef50-140">Místní servery jsou automaticky vyváženy během několika minut po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="bef50-140">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="bef50-141">Místní servery však můžete také ručně vyvážit přihlašují k headnode clusteru a spuštěním následujících příkazů z okna příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="bef50-141">However, you can also manually balance the regional servers by logging in to the headnode of cluster and running the following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="bef50-142">Storm</span><span class="sxs-lookup"><span data-stu-id="bef50-142">Storm</span></span>

    <span data-ttu-id="bef50-143">Můžete bezproblémově přidávat nebo odebírat uzly dat do clusteru Storm, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="bef50-143">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="bef50-144">Ale po úspěšném dokončení operace škálování, budete muset znovu vyvážit topologii.</span><span class="sxs-lookup"><span data-stu-id="bef50-144">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>

    <span data-ttu-id="bef50-145">Vyrovnává lze dosáhnout dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="bef50-145">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="bef50-146">Storm webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="bef50-146">Storm web UI</span></span>
  * <span data-ttu-id="bef50-147">Nástroj pro rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="bef50-147">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="bef50-148">Podrobnosti najdete [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bef50-148">Please refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="bef50-149">Uživatelské rozhraní Storm webu je k dispozici v clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="bef50-149">The Storm web UI is available on the HDInsight cluster:</span></span>

    ![Obnovte rovnováhu škálování HDInsight storm](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    <span data-ttu-id="bef50-151">Tady je příklad jak znovu vyvážit topologie Storm pomocí rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="bef50-151">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="bef50-152">Chcete-li změnit velikost clusteru Hadoop pomocí prostředí Azure PowerShell, spusťte následující příkaz z klientský počítač:</span><span class="sxs-lookup"><span data-stu-id="bef50-152">To change the Hadoop cluster size by using Azure PowerShell, run the following command from a client machine:</span></span>

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a><span data-ttu-id="bef50-153">Udělení nebo odvolání přístupu</span><span class="sxs-lookup"><span data-stu-id="bef50-153">Grant/revoke access</span></span>
<span data-ttu-id="bef50-154">Clustery HDInsight mají následující webové služby HTTP (všechny tyto služby mají RESTful koncových bodů):</span><span class="sxs-lookup"><span data-stu-id="bef50-154">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="bef50-155">ODBC</span><span class="sxs-lookup"><span data-stu-id="bef50-155">ODBC</span></span>
* <span data-ttu-id="bef50-156">JDBC</span><span class="sxs-lookup"><span data-stu-id="bef50-156">JDBC</span></span>
* <span data-ttu-id="bef50-157">Ambari</span><span class="sxs-lookup"><span data-stu-id="bef50-157">Ambari</span></span>
* <span data-ttu-id="bef50-158">Oozie</span><span class="sxs-lookup"><span data-stu-id="bef50-158">Oozie</span></span>
* <span data-ttu-id="bef50-159">Templeton</span><span class="sxs-lookup"><span data-stu-id="bef50-159">Templeton</span></span>

<span data-ttu-id="bef50-160">Ve výchozím nastavení jsou tyto služby oprávnění pro přístup.</span><span class="sxs-lookup"><span data-stu-id="bef50-160">By default, these services are granted for access.</span></span> <span data-ttu-id="bef50-161">Vám může odvolání nebo udělit přístup.</span><span class="sxs-lookup"><span data-stu-id="bef50-161">You can revoke/grant the access.</span></span> <span data-ttu-id="bef50-162">K odvolání:</span><span class="sxs-lookup"><span data-stu-id="bef50-162">To revoke:</span></span>

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

<span data-ttu-id="bef50-163">Udělit:</span><span class="sxs-lookup"><span data-stu-id="bef50-163">To grant:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> <span data-ttu-id="bef50-164">Pomocí udělení nebo odvolání přístupu, obnoví clusteru uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="bef50-164">By granting/revoking the access, you will reset the cluster user name and password.</span></span>
>
>

<span data-ttu-id="bef50-165">To lze provést také prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="bef50-165">This can also be done via the Portal.</span></span> <span data-ttu-id="bef50-166">V tématu [spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="bef50-166">See [Administer HDInsight by using the Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="bef50-167">Aktualizovat pověření uživatele HTTP</span><span class="sxs-lookup"><span data-stu-id="bef50-167">Update HTTP user credentials</span></span>
<span data-ttu-id="bef50-168">Je stejným způsobem jako [HTTP udělení nebo odvolání přístupu](#grant/revoke-access). Pokud cluster byl přidělen přístup protokolu HTTP, musí se nejdřív odvolat.</span><span class="sxs-lookup"><span data-stu-id="bef50-168">It is the same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If the cluster has been granted the HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="bef50-169">A pak udělují přístup s novými pověřeními uživatele HTTP.</span><span class="sxs-lookup"><span data-stu-id="bef50-169">And then grant the access with new HTTP user credentials.</span></span>

## <a name="find-the-default-storage-account"></a><span data-ttu-id="bef50-170">Najít výchozí účet úložiště</span><span class="sxs-lookup"><span data-stu-id="bef50-170">Find the default storage account</span></span>
<span data-ttu-id="bef50-171">Následující skript prostředí Powershell ukazuje, jak získat výchozí název účtu úložiště a výchozí klíč účtu úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="bef50-171">The following Powershell script demonstrates how to get the default storage account name and the default storage account key for a cluster.</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-the-resource-group"></a><span data-ttu-id="bef50-172">Najít skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="bef50-172">Find the resource group</span></span>
<span data-ttu-id="bef50-173">V režimu Resource Manager každý cluster HDInsight patří do skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bef50-173">In the Resource Manager mode, each HDInsight cluster belongs to an Azure resource group.</span></span>  <span data-ttu-id="bef50-174">Chcete-li najít skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="bef50-174">To find the resource group:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a><span data-ttu-id="bef50-175">Odesílání úloh</span><span class="sxs-lookup"><span data-stu-id="bef50-175">Submit jobs</span></span>
<span data-ttu-id="bef50-176">**K odesílání úloh MapReduce**</span><span class="sxs-lookup"><span data-stu-id="bef50-176">**To submit MapReduce jobs**</span></span>

<span data-ttu-id="bef50-177">V tématu [ukázky spustit Hadoop MapReduce v HDInsight se systémem Windows](hdinsight-run-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bef50-177">See [Run Hadoop MapReduce samples in Windows-based HDInsight](hdinsight-run-samples.md).</span></span>

<span data-ttu-id="bef50-178">**K odeslání úloh Hive**</span><span class="sxs-lookup"><span data-stu-id="bef50-178">**To submit Hive jobs**</span></span>

<span data-ttu-id="bef50-179">V tématu [spouštění dotazů Hive pomocí prostředí PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="bef50-179">See [Run Hive queries using PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span></span>

<span data-ttu-id="bef50-180">**K odeslání úlohy Pig**</span><span class="sxs-lookup"><span data-stu-id="bef50-180">**To submit Pig jobs**</span></span>

<span data-ttu-id="bef50-181">V tématu [úlohy Pig spuštění pomocí prostředí PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="bef50-181">See [Run Pig jobs using PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span></span>

<span data-ttu-id="bef50-182">**K odesílání úloh Sqoop**</span><span class="sxs-lookup"><span data-stu-id="bef50-182">**To submit Sqoop jobs**</span></span>

<span data-ttu-id="bef50-183">V tématu [použití nástroje Sqoop se HDInsight](hdinsight-use-sqoop.md).</span><span class="sxs-lookup"><span data-stu-id="bef50-183">See [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).</span></span>

<span data-ttu-id="bef50-184">**K odesílání úloh Oozie**</span><span class="sxs-lookup"><span data-stu-id="bef50-184">**To submit Oozie jobs**</span></span>

<span data-ttu-id="bef50-185">V tématu [Oozie použití se systémem Hadoop k definování a spuštění workflowu v HDInsight](hdinsight-use-oozie.md).</span><span class="sxs-lookup"><span data-stu-id="bef50-185">See [Use Oozie with Hadoop to define and run a workflow in HDInsight](hdinsight-use-oozie.md).</span></span>

## <a name="upload-data-to-azure-blob-storage"></a><span data-ttu-id="bef50-186">Nahrání dat do úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="bef50-186">Upload data to Azure Blob storage</span></span>
<span data-ttu-id="bef50-187">Viz [Nahrání dat do služby HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="bef50-187">See [Upload data to HDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="bef50-188">Viz také</span><span class="sxs-lookup"><span data-stu-id="bef50-188">See Also</span></span>
* <span data-ttu-id="bef50-189">[HDInsight rutiny referenční dokumentaci k nástroji][hdinsight-powershell-reference]</span><span class="sxs-lookup"><span data-stu-id="bef50-189">[HDInsight cmdlet reference documentation][hdinsight-powershell-reference]</span></span>
* <span data-ttu-id="bef50-190">[Spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="bef50-190">[Administer HDInsight by using the Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="bef50-191">[Spravovat HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="bef50-191">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="bef50-192">[Vytvoření clusterů HDInsight][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="bef50-192">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="bef50-193">[Nahrání dat do služby HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="bef50-193">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="bef50-194">[Odesílání úloh Hadoop prostřednictvím kódu programu][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="bef50-194">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="bef50-195">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="bef50-195">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

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
