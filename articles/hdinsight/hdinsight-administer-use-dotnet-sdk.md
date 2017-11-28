---
title: "aaaManage Hadoop clusterů v HDInsight pomocí .NET SDK - Azure | Microsoft Docs"
description: "Zjistěte, jak tooperform administrativní úlohy pro hello clusterů systému Hadoop v HDInsight pomocí sady .NET SDK HDInsight."
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
ms.openlocfilehash: d8bbf966b7eba3e943dfb2f764d15d8e52b9be71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a><span data-ttu-id="aaea6-103">Správa clusterů systému Hadoop v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="aaea6-103">Manage Hadoop clusters in HDInsight by using .NET SDK</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="aaea6-104">Zjistěte, jak toomanage HDInsight clusterů pomocí [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="aaea6-104">Learn how toomanage HDInsight clusters using [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>

<span data-ttu-id="aaea6-105">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="aaea6-105">**Prerequisites**</span></span>

<span data-ttu-id="aaea6-106">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="aaea6-106">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="aaea6-107">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="aaea6-107">**An Azure subscription**.</span></span> <span data-ttu-id="aaea6-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="aaea6-108">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="connect-tooazure-hdinsight"></a><span data-ttu-id="aaea6-109">Připojit tooAzure HDInsight</span><span class="sxs-lookup"><span data-stu-id="aaea6-109">Connect tooAzure HDInsight</span></span>

<span data-ttu-id="aaea6-110">Je třeba hello následující balíčky Nuget:</span><span class="sxs-lookup"><span data-stu-id="aaea6-110">You need hello following Nuget packages:</span></span>

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

<span data-ttu-id="aaea6-111">Hello následující příklad kódu ukazuje, jak tooconnect tooAzure než budete moct spravovat HDInsight clustery v rámci vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="aaea6-111">hello following code sample shows you how tooconnect tooAzure before you can administer HDInsight clusters under your Azure subscription.</span></span>

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
            // This is hello GUID for hello PowerShell client. Used for interactive logins in this example.
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

                System.Console.WriteLine("Press ENTER toocontinue");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate tooan Azure subscription and retrieve an authentication token
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
                // Create a client for hello Resource manager and set hello subscription ID
                var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                resourceManagementClient.SubscriptionId = SubscriptionId;
                // Register hello HDInsight provider
                var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
            }
        }
    }

<span data-ttu-id="aaea6-112">Zobrazí se výzva a při spuštění tohoto programu.</span><span class="sxs-lookup"><span data-stu-id="aaea6-112">You shall see a prompt when you run this program.</span></span>  <span data-ttu-id="aaea6-113">Pokud nechcete, aby toosee hello řádku, přečtěte si téma [vytvořit neinteraktivního ověřování aplikace .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="aaea6-113">If you don't want toosee hello prompt, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

## <a name="create-clusters"></a><span data-ttu-id="aaea6-114">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="aaea6-114">Create clusters</span></span>
<span data-ttu-id="aaea6-115">V tématu [hello vytvořit systémem Linux clusterů v HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="aaea6-115">See [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="aaea6-116">Seznam clustery</span><span class="sxs-lookup"><span data-stu-id="aaea6-116">List clusters</span></span>
<span data-ttu-id="aaea6-117">Hello následující fragment kódu obsahuje clustery a některé vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="aaea6-117">hello following code snippet lists clusters and some properties:</span></span>

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a><span data-ttu-id="aaea6-118">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="aaea6-118">Delete clusters</span></span>
<span data-ttu-id="aaea6-119">Použijte následující toodelete fragmentu kódu cluster synchronně nebo asynchronně hello:</span><span class="sxs-lookup"><span data-stu-id="aaea6-119">Use hello following code snippet toodelete a cluster synchronously or asynchronously:</span></span> 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a><span data-ttu-id="aaea6-120">Škálování clusterů</span><span class="sxs-lookup"><span data-stu-id="aaea6-120">Scale clusters</span></span>
<span data-ttu-id="aaea6-121">Hello clusteru škálování funkce vám umožní toochange hello počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti toore – vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="aaea6-121">hello cluster scaling feature allows you toochange hello number of worker nodes used by a cluster that is running in Azure HDInsight without having toore-create hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="aaea6-122">Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="aaea6-122">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="aaea6-123">Pokud si nejste jistí hello verze vašeho clusteru, můžete zkontrolovat hello stránku vlastností.</span><span class="sxs-lookup"><span data-stu-id="aaea6-123">If you are unsure of hello version of your cluster, you can check hello Properties page.</span></span>  <span data-ttu-id="aaea6-124">V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="aaea6-124">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
> 
> 

<span data-ttu-id="aaea6-125">Hello dopad změny hello počet uzlů dat pro každý typ clusteru podporuje HDInsight:</span><span class="sxs-lookup"><span data-stu-id="aaea6-125">hello impact of changing hello number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="aaea6-126">Hadoop</span><span class="sxs-lookup"><span data-stu-id="aaea6-126">Hadoop</span></span>
  
    <span data-ttu-id="aaea6-127">Můžete zvýšit bezproblémově hello počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné.</span><span class="sxs-lookup"><span data-stu-id="aaea6-127">You can seamlessly increase hello number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="aaea6-128">Nové úlohy můžete také odeslány, když probíhá operace hello.</span><span class="sxs-lookup"><span data-stu-id="aaea6-128">New jobs can also be submitted while hello operation is in progress.</span></span> <span data-ttu-id="aaea6-129">Selhání v rámci operace škálování pohodlné zpracování tak, aby hello clusteru vždy zůstává ve funkčním stavu.</span><span class="sxs-lookup"><span data-stu-id="aaea6-129">Failures in a scaling operation are gracefully handled so that hello cluster is always left in a functional state.</span></span>
  
    <span data-ttu-id="aaea6-130">Pokud se Hadoop cluster měřítko snížením hello počet uzlů data, některé služby hello v clusteru hello restartovat.</span><span class="sxs-lookup"><span data-stu-id="aaea6-130">When a Hadoop cluster is scaled down by reducing hello number of data nodes, some of hello services in hello cluster are restarted.</span></span> <span data-ttu-id="aaea6-131">To způsobí, že všechny spuštěné a čeká se na úlohy toofail na dokončení hello hello operace škálování.</span><span class="sxs-lookup"><span data-stu-id="aaea6-131">This causes all running and pending jobs toofail at hello completion of hello scaling operation.</span></span> <span data-ttu-id="aaea6-132">Po dokončení operace hello může, ale odešlete znovu hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="aaea6-132">You can, however, resubmit hello jobs once hello operation is complete.</span></span>
* <span data-ttu-id="aaea6-133">HBase</span><span class="sxs-lookup"><span data-stu-id="aaea6-133">HBase</span></span>
  
    <span data-ttu-id="aaea6-134">Můžete bezproblémově přidat nebo odebrat uzly clusteru HBase tooyour, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="aaea6-134">You can seamlessly add or remove nodes tooyour HBase cluster while it is running.</span></span> <span data-ttu-id="aaea6-135">Místní servery jsou automaticky vyváženy během několika minut od hello operace škálování.</span><span class="sxs-lookup"><span data-stu-id="aaea6-135">Regional Servers are automatically balanced within a few minutes of completing hello scaling operation.</span></span> <span data-ttu-id="aaea6-136">Po přihlášení do hello headnode clusteru a spuštěné hello následujících příkazů z okna příkazového řádku však můžete také ručně vyvážit hello místní servery:</span><span class="sxs-lookup"><span data-stu-id="aaea6-136">However, you can also manually balance hello regional servers by logging into hello headnode of cluster and running hello following commands from a command prompt window:</span></span>
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="aaea6-137">Storm</span><span class="sxs-lookup"><span data-stu-id="aaea6-137">Storm</span></span>
  
    <span data-ttu-id="aaea6-138">Můžete bez problémů přidat nebo odebrat cluster Storm tooyour uzly dat je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="aaea6-138">You can seamlessly add or remove data nodes tooyour Storm cluster while it is running.</span></span> <span data-ttu-id="aaea6-139">Ale po úspěšném dokončení hello operace škálování, budete potřebovat toorebalance hello topologie.</span><span class="sxs-lookup"><span data-stu-id="aaea6-139">But after a successful completion of hello scaling operation, you will need toorebalance hello topology.</span></span>
  
    <span data-ttu-id="aaea6-140">Vyrovnává lze dosáhnout dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="aaea6-140">Rebalancing can be accomplished in two ways:</span></span>
  
  * <span data-ttu-id="aaea6-141">Storm webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="aaea6-141">Storm web UI</span></span>
  * <span data-ttu-id="aaea6-142">Nástroj pro rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="aaea6-142">Command-line interface (CLI) tool</span></span>
    
    <span data-ttu-id="aaea6-143">Podrobnosti najdete toohello [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="aaea6-143">Please refer toohello [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>
    
    <span data-ttu-id="aaea6-144">Hello Storm webového uživatelského rozhraní není k dispozici v clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="aaea6-144">hello Storm web UI is available on hello HDInsight cluster:</span></span>
    
    ![Obnovte rovnováhu škálování Storm v HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    <span data-ttu-id="aaea6-146">Tady je příklad jak toouse hello rozhraní příkazového řádku příkaz topologie Storm toorebalance hello:</span><span class="sxs-lookup"><span data-stu-id="aaea6-146">Here is an example how toouse hello CLI command toorebalance hello Storm topology:</span></span>
    
        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="aaea6-147">Následující kód fragment kódu ukazuje, jak Hello tooresize cluster synchronně nebo asynchronně:</span><span class="sxs-lookup"><span data-stu-id="aaea6-147">hello following code snippet shows how tooresize a cluster synchronously or asynchronously:</span></span>

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a><span data-ttu-id="aaea6-148">Udělení nebo odvolání přístupu</span><span class="sxs-lookup"><span data-stu-id="aaea6-148">Grant/revoke access</span></span>
<span data-ttu-id="aaea6-149">Clustery HDInsight mít hello následující HTTP webové služby (všechny tyto služby mají RESTful koncových bodů):</span><span class="sxs-lookup"><span data-stu-id="aaea6-149">HDInsight clusters have hello following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="aaea6-150">ODBC</span><span class="sxs-lookup"><span data-stu-id="aaea6-150">ODBC</span></span>
* <span data-ttu-id="aaea6-151">JDBC</span><span class="sxs-lookup"><span data-stu-id="aaea6-151">JDBC</span></span>
* <span data-ttu-id="aaea6-152">Ambari</span><span class="sxs-lookup"><span data-stu-id="aaea6-152">Ambari</span></span>
* <span data-ttu-id="aaea6-153">Oozie</span><span class="sxs-lookup"><span data-stu-id="aaea6-153">Oozie</span></span>
* <span data-ttu-id="aaea6-154">Templeton</span><span class="sxs-lookup"><span data-stu-id="aaea6-154">Templeton</span></span>

<span data-ttu-id="aaea6-155">Ve výchozím nastavení jsou tyto služby oprávnění pro přístup.</span><span class="sxs-lookup"><span data-stu-id="aaea6-155">By default, these services are granted for access.</span></span> <span data-ttu-id="aaea6-156">Můžete můžete odvolat nebo udělit přístup hello.</span><span class="sxs-lookup"><span data-stu-id="aaea6-156">You can revoke/grant hello access.</span></span> <span data-ttu-id="aaea6-157">toorevoke:</span><span class="sxs-lookup"><span data-stu-id="aaea6-157">toorevoke:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

<span data-ttu-id="aaea6-158">toogrant:</span><span class="sxs-lookup"><span data-stu-id="aaea6-158">toogrant:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> <span data-ttu-id="aaea6-159">Pomocí udělení nebo odvolání přístupu hello, obnoví hello clusteru uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="aaea6-159">By granting/revoking hello access, you will reset hello cluster user name and password.</span></span>
> 
> 

<span data-ttu-id="aaea6-160">To lze provést také prostřednictvím hello portálu.</span><span class="sxs-lookup"><span data-stu-id="aaea6-160">This can also be done via hello Portal.</span></span> <span data-ttu-id="aaea6-161">V tématu [hello spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="aaea6-161">See [Administer HDInsight by using hello Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="aaea6-162">Aktualizovat pověření uživatele HTTP</span><span class="sxs-lookup"><span data-stu-id="aaea6-162">Update HTTP user credentials</span></span>
<span data-ttu-id="aaea6-163">Je hello stejný postup jako [HTTP udělení nebo odvolání přístupu](#grant/revoke-access). Pokud hello clusteru byla udělena hello přístup protokolu HTTP, musí se nejdřív odvolat.</span><span class="sxs-lookup"><span data-stu-id="aaea6-163">It is hello same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If hello cluster has been granted hello HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="aaea6-164">A pak udělují přístup hello s novými pověřeními uživatele HTTP.</span><span class="sxs-lookup"><span data-stu-id="aaea6-164">And then grant hello access with new HTTP user credentials.</span></span>

## <a name="find-hello-default-storage-account"></a><span data-ttu-id="aaea6-165">Najít hello výchozí účet úložiště</span><span class="sxs-lookup"><span data-stu-id="aaea6-165">Find hello default storage account</span></span>
<span data-ttu-id="aaea6-166">Hello následující fragment kódu ukazuje, jak tooget hello výchozí název účtu úložiště a hello výchozí klíč účtu úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="aaea6-166">hello following code snippet demonstrates how tooget hello default storage account name and hello default storage account key for a cluster.</span></span>

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a><span data-ttu-id="aaea6-167">Odesílání úloh</span><span class="sxs-lookup"><span data-stu-id="aaea6-167">Submit jobs</span></span>
<span data-ttu-id="aaea6-168">**úlohy MapReduce toosubmit**</span><span class="sxs-lookup"><span data-stu-id="aaea6-168">**toosubmit MapReduce jobs**</span></span>

<span data-ttu-id="aaea6-169">V tématu [ukázky spustit Hadoop MapReduce v HDInsight](hdinsight-hadoop-run-samples-linux.md).</span><span class="sxs-lookup"><span data-stu-id="aaea6-169">See [Run Hadoop MapReduce samples in HDInsight](hdinsight-hadoop-run-samples-linux.md).</span></span>

<span data-ttu-id="aaea6-170">**toosubmit úloh Hive**</span><span class="sxs-lookup"><span data-stu-id="aaea6-170">**toosubmit Hive jobs**</span></span> 

<span data-ttu-id="aaea6-171">V tématu [spouštění dotazů Hive pomocí sady .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="aaea6-171">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>

<span data-ttu-id="aaea6-172">**úlohy Pig toosubmit**</span><span class="sxs-lookup"><span data-stu-id="aaea6-172">**toosubmit Pig jobs**</span></span>

<span data-ttu-id="aaea6-173">V tématu [úlohy spustit Pig pomocí sady .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="aaea6-173">See [Run Pig jobs using .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span></span>

<span data-ttu-id="aaea6-174">**toosubmit Sqoop úlohy**</span><span class="sxs-lookup"><span data-stu-id="aaea6-174">**toosubmit Sqoop jobs**</span></span>

<span data-ttu-id="aaea6-175">V tématu [použití nástroje Sqoop se HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="aaea6-175">See [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span></span>

<span data-ttu-id="aaea6-176">**toosubmit Oozie úlohy**</span><span class="sxs-lookup"><span data-stu-id="aaea6-176">**toosubmit Oozie jobs**</span></span>

<span data-ttu-id="aaea6-177">V tématu [Oozie použití s Hadoop toodefine a spouštění pracovních postupů v prostředí HDInsight](hdinsight-use-oozie-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="aaea6-177">See [Use Oozie with Hadoop toodefine and run a workflow in HDInsight](hdinsight-use-oozie-linux-mac.md).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="aaea6-178">Nahrání dat tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="aaea6-178">Upload data tooAzure Blob storage</span></span>
<span data-ttu-id="aaea6-179">V tématu [nahrát data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="aaea6-179">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="aaea6-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="aaea6-180">See Also</span></span>
* [<span data-ttu-id="aaea6-181">HDInsight .NET SDK referenční dokumentaci k nástroji</span><span class="sxs-lookup"><span data-stu-id="aaea6-181">HDInsight .NET SDK reference documentation</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* <span data-ttu-id="aaea6-182">[Spravovat HDInsight pomocí hello portálu Azure][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="aaea6-182">[Administer HDInsight by using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="aaea6-183">[Spravovat HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="aaea6-183">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="aaea6-184">[Vytvoření clusterů HDInsight][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="aaea6-184">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="aaea6-185">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="aaea6-185">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="aaea6-186">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="aaea6-186">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

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


