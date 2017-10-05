---
title: "Správa clusterů systému Hadoop v HDInsight pomocí .NET SDK - Azure | Microsoft Docs"
description: "Zjistěte, jak k provádění úloh správy pro clusterů systému Hadoop v HDInsight pomocí sady .NET SDK HDInsight."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: fd134765-c2a0-488a-bca6-184d814d78e9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: c10471425fa1202ddb7fe35d0adf4ef33509f268
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a><span data-ttu-id="d833c-103">Správa clusterů systému Hadoop v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="d833c-103">Manage Hadoop clusters in HDInsight by using .NET SDK</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="d833c-104">Zjistěte, jak Správa clusterů HDInsight pomocí [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="d833c-104">Learn how to manage HDInsight clusters using [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>

<span data-ttu-id="d833c-105">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="d833c-105">**Prerequisites**</span></span>

<span data-ttu-id="d833c-106">Je nutné, abyste před zahájením tohoto článku měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="d833c-106">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="d833c-107">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="d833c-107">**An Azure subscription**.</span></span> <span data-ttu-id="d833c-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="d833c-108">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="connect-to-azure-hdinsight"></a><span data-ttu-id="d833c-109">Připojení k Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d833c-109">Connect to Azure HDInsight</span></span>

<span data-ttu-id="d833c-110">Budete potřebovat následující balíčky Nuget:</span><span class="sxs-lookup"><span data-stu-id="d833c-110">You need the following Nuget packages:</span></span>

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

<span data-ttu-id="d833c-111">Následující příklad kódu ukazuje, jak se připojit k Azure, než budete moct spravovat clustery HDInsight v rámci vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="d833c-111">The following code sample shows you how to connect to Azure before you can administer HDInsight clusters under your Azure subscription.</span></span>

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate to an Azure subscription and retrieve an authentication token
            /// </summary>
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
        }
    }

<span data-ttu-id="d833c-112">Zobrazí se výzva a při spuštění tohoto programu.</span><span class="sxs-lookup"><span data-stu-id="d833c-112">You shall see a prompt when you run this program.</span></span>  <span data-ttu-id="d833c-113">Pokud nechcete zobrazí výzva, přečtěte si téma [vytvořit neinteraktivního ověřování aplikace .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="d833c-113">If you don't want to see the prompt, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

## <a name="create-clusters"></a><span data-ttu-id="d833c-114">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="d833c-114">Create clusters</span></span>
<span data-ttu-id="d833c-115">V tématu [vytvořit systémem Linux clusterů v HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="d833c-115">See [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="d833c-116">Seznam clustery</span><span class="sxs-lookup"><span data-stu-id="d833c-116">List clusters</span></span>
<span data-ttu-id="d833c-117">Následující fragment kódu obsahuje clustery a některé vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="d833c-117">The following code snippet lists clusters and some properties:</span></span>

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a><span data-ttu-id="d833c-118">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="d833c-118">Delete clusters</span></span>
<span data-ttu-id="d833c-119">Pomocí následující fragment kódu můžete odstranit cluster synchronně nebo asynchronně:</span><span class="sxs-lookup"><span data-stu-id="d833c-119">Use the following code snippet to delete a cluster synchronously or asynchronously:</span></span> 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a><span data-ttu-id="d833c-120">Škálování clusterů</span><span class="sxs-lookup"><span data-stu-id="d833c-120">Scale clusters</span></span>
<span data-ttu-id="d833c-121">Funkce škálování clusteru umožňuje změnit počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti znovu vytvořit cluster.</span><span class="sxs-lookup"><span data-stu-id="d833c-121">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d833c-122">Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="d833c-122">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="d833c-123">Pokud si nejste jistí na verzi vašeho clusteru, můžete zkontrolovat stránku vlastností.</span><span class="sxs-lookup"><span data-stu-id="d833c-123">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="d833c-124">V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="d833c-124">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
> 
> 

<span data-ttu-id="d833c-125">Dopad změny v počtu uzlů dat pro každý typ clusteru podporuje HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d833c-125">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="d833c-126">Hadoop</span><span class="sxs-lookup"><span data-stu-id="d833c-126">Hadoop</span></span>
  
    <span data-ttu-id="d833c-127">Můžete bez problémů zvýšit počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné.</span><span class="sxs-lookup"><span data-stu-id="d833c-127">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="d833c-128">Nové úlohy můžete také odeslány, když probíhá operace.</span><span class="sxs-lookup"><span data-stu-id="d833c-128">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="d833c-129">Selhání v rámci operace škálování pohodlné zpracování tak, aby cluster zůstane vždy ve funkčním stavu.</span><span class="sxs-lookup"><span data-stu-id="d833c-129">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>
  
    <span data-ttu-id="d833c-130">Pokud se Hadoop cluster měřítko snížením počtu uzlů data, některé služby v clusteru restartovat.</span><span class="sxs-lookup"><span data-stu-id="d833c-130">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="d833c-131">To způsobí, že všechny spuštěné a čeká se na úlohy selhání po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="d833c-131">This causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="d833c-132">Můžete, ale odešlete znovu úloh po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="d833c-132">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="d833c-133">HBase</span><span class="sxs-lookup"><span data-stu-id="d833c-133">HBase</span></span>
  
    <span data-ttu-id="d833c-134">Bezproblémově můžete přidávat nebo odebírat uzly do clusteru HBase, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="d833c-134">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="d833c-135">Místní servery jsou automaticky vyváženy během několika minut po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="d833c-135">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="d833c-136">Protokolování do headnode clusteru a spuštěním následujících příkazů z okna příkazového řádku však můžete také ručně vyvážit místní servery:</span><span class="sxs-lookup"><span data-stu-id="d833c-136">However, you can also manually balance the regional servers by logging into the headnode of cluster and running the following commands from a command prompt window:</span></span>
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="d833c-137">Storm</span><span class="sxs-lookup"><span data-stu-id="d833c-137">Storm</span></span>
  
    <span data-ttu-id="d833c-138">Můžete bezproblémově přidávat nebo odebírat uzly dat do clusteru Storm, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="d833c-138">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="d833c-139">Ale po úspěšném dokončení operace škálování, budete muset znovu vyvážit topologii.</span><span class="sxs-lookup"><span data-stu-id="d833c-139">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>
  
    <span data-ttu-id="d833c-140">Vyrovnává lze dosáhnout dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="d833c-140">Rebalancing can be accomplished in two ways:</span></span>
  
  * <span data-ttu-id="d833c-141">Storm webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d833c-141">Storm web UI</span></span>
  * <span data-ttu-id="d833c-142">Nástroj pro rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="d833c-142">Command-line interface (CLI) tool</span></span>
    
    <span data-ttu-id="d833c-143">Podrobnosti najdete [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d833c-143">Please refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>
    
    <span data-ttu-id="d833c-144">Uživatelské rozhraní Storm webu je k dispozici v clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d833c-144">The Storm web UI is available on the HDInsight cluster:</span></span>
    
    ![Obnovte rovnováhu škálování Storm v HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    <span data-ttu-id="d833c-146">Tady je příklad jak znovu vyvážit topologie Storm pomocí rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="d833c-146">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>
    
        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="d833c-147">Následující fragment kódu ukazuje, jak změnit velikost clusteru s podporou synchronně nebo asynchronně:</span><span class="sxs-lookup"><span data-stu-id="d833c-147">The following code snippet shows how to resize a cluster synchronously or asynchronously:</span></span>

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a><span data-ttu-id="d833c-148">Udělení nebo odvolání přístupu</span><span class="sxs-lookup"><span data-stu-id="d833c-148">Grant/revoke access</span></span>
<span data-ttu-id="d833c-149">Clustery HDInsight mají následující webové služby HTTP (všechny tyto služby mají RESTful koncových bodů):</span><span class="sxs-lookup"><span data-stu-id="d833c-149">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="d833c-150">ODBC</span><span class="sxs-lookup"><span data-stu-id="d833c-150">ODBC</span></span>
* <span data-ttu-id="d833c-151">JDBC</span><span class="sxs-lookup"><span data-stu-id="d833c-151">JDBC</span></span>
* <span data-ttu-id="d833c-152">Ambari</span><span class="sxs-lookup"><span data-stu-id="d833c-152">Ambari</span></span>
* <span data-ttu-id="d833c-153">Oozie</span><span class="sxs-lookup"><span data-stu-id="d833c-153">Oozie</span></span>
* <span data-ttu-id="d833c-154">Templeton</span><span class="sxs-lookup"><span data-stu-id="d833c-154">Templeton</span></span>

<span data-ttu-id="d833c-155">Ve výchozím nastavení jsou tyto služby oprávnění pro přístup.</span><span class="sxs-lookup"><span data-stu-id="d833c-155">By default, these services are granted for access.</span></span> <span data-ttu-id="d833c-156">Vám může odvolání nebo udělit přístup.</span><span class="sxs-lookup"><span data-stu-id="d833c-156">You can revoke/grant the access.</span></span> <span data-ttu-id="d833c-157">K odvolání:</span><span class="sxs-lookup"><span data-stu-id="d833c-157">To revoke:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

<span data-ttu-id="d833c-158">Udělit:</span><span class="sxs-lookup"><span data-stu-id="d833c-158">To grant:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> <span data-ttu-id="d833c-159">Pomocí udělení nebo odvolání přístupu, obnoví clusteru uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="d833c-159">By granting/revoking the access, you will reset the cluster user name and password.</span></span>
> 
> 

<span data-ttu-id="d833c-160">To lze provést také prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="d833c-160">This can also be done via the Portal.</span></span> <span data-ttu-id="d833c-161">V tématu [spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="d833c-161">See [Administer HDInsight by using the Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="d833c-162">Aktualizovat pověření uživatele HTTP</span><span class="sxs-lookup"><span data-stu-id="d833c-162">Update HTTP user credentials</span></span>
<span data-ttu-id="d833c-163">Je stejným způsobem jako [HTTP udělení nebo odvolání přístupu](#grant/revoke-access). Pokud cluster byl přidělen přístup protokolu HTTP, musí se nejdřív odvolat.</span><span class="sxs-lookup"><span data-stu-id="d833c-163">It is the same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If the cluster has been granted the HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="d833c-164">A pak udělují přístup s novými pověřeními uživatele HTTP.</span><span class="sxs-lookup"><span data-stu-id="d833c-164">And then grant the access with new HTTP user credentials.</span></span>

## <a name="find-the-default-storage-account"></a><span data-ttu-id="d833c-165">Najít výchozí účet úložiště</span><span class="sxs-lookup"><span data-stu-id="d833c-165">Find the default storage account</span></span>
<span data-ttu-id="d833c-166">Následující fragment kódu ukazuje, jak získat výchozí název účtu úložiště a výchozí klíč účtu úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d833c-166">The following code snippet demonstrates how to get the default storage account name and the default storage account key for a cluster.</span></span>

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a><span data-ttu-id="d833c-167">Odesílání úloh</span><span class="sxs-lookup"><span data-stu-id="d833c-167">Submit jobs</span></span>
<span data-ttu-id="d833c-168">**K odesílání úloh MapReduce**</span><span class="sxs-lookup"><span data-stu-id="d833c-168">**To submit MapReduce jobs**</span></span>

<span data-ttu-id="d833c-169">V tématu [ukázky spustit Hadoop MapReduce v HDInsight](hdinsight-hadoop-run-samples-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d833c-169">See [Run Hadoop MapReduce samples in HDInsight](hdinsight-hadoop-run-samples-linux.md).</span></span>

<span data-ttu-id="d833c-170">**K odeslání úloh Hive**</span><span class="sxs-lookup"><span data-stu-id="d833c-170">**To submit Hive jobs**</span></span> 

<span data-ttu-id="d833c-171">V tématu [spouštění dotazů Hive pomocí sady .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d833c-171">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>

<span data-ttu-id="d833c-172">**K odeslání úlohy Pig**</span><span class="sxs-lookup"><span data-stu-id="d833c-172">**To submit Pig jobs**</span></span>

<span data-ttu-id="d833c-173">V tématu [úlohy spustit Pig pomocí sady .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d833c-173">See [Run Pig jobs using .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span></span>

<span data-ttu-id="d833c-174">**K odesílání úloh Sqoop**</span><span class="sxs-lookup"><span data-stu-id="d833c-174">**To submit Sqoop jobs**</span></span>

<span data-ttu-id="d833c-175">V tématu [použití nástroje Sqoop se HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d833c-175">See [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span></span>

<span data-ttu-id="d833c-176">**K odesílání úloh Oozie**</span><span class="sxs-lookup"><span data-stu-id="d833c-176">**To submit Oozie jobs**</span></span>

<span data-ttu-id="d833c-177">V tématu [Oozie použití se systémem Hadoop k definování a spuštění workflowu v HDInsight](hdinsight-use-oozie-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="d833c-177">See [Use Oozie with Hadoop to define and run a workflow in HDInsight](hdinsight-use-oozie-linux-mac.md).</span></span>

## <a name="upload-data-to-azure-blob-storage"></a><span data-ttu-id="d833c-178">Nahrání dat do úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="d833c-178">Upload data to Azure Blob storage</span></span>
<span data-ttu-id="d833c-179">Viz [Nahrání dat do služby HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="d833c-179">See [Upload data to HDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="d833c-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="d833c-180">See Also</span></span>
* [<span data-ttu-id="d833c-181">HDInsight .NET SDK referenční dokumentaci k nástroji</span><span class="sxs-lookup"><span data-stu-id="d833c-181">HDInsight .NET SDK reference documentation</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* <span data-ttu-id="d833c-182">[Spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="d833c-182">[Administer HDInsight by using the Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="d833c-183">[Spravovat HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="d833c-183">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="d833c-184">[Vytvoření clusterů HDInsight][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="d833c-184">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="d833c-185">[Nahrání dat do služby HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="d833c-185">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="d833c-186">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="d833c-186">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


