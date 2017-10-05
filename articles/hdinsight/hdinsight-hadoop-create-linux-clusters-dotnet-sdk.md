---
title: "Vytváření clusterů systému Hadoop pomocí rozhraní .NET – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak vytvořit clustery Hadoop, HBase, Storm a Spark pro HDInsight pomocí sady .NET SDK HDInsight v systému Linux."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9c74e3dc-837f-4c90-bbb1-489bc7124a3d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/17/2017
ms.author: jgao
ms.openlocfilehash: ccd3a0c777510e0694170b2f9acc8da0e7dcde9b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-net-sdk"></a><span data-ttu-id="8a9f5-103">Vytvořit clustery se systémem Linux v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8a9f5-103">Create Linux-based clusters in HDInsight using the .NET SDK</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]


<span data-ttu-id="8a9f5-104">Zjistěte, jak vytvořit Hadoop cluster v clusteru Azure HDInsight pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-104">Learn how to create a Hadoop cluster in Azure HDInsight cluster using the .NET SDK.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8a9f5-105">Kroky v tomto dokumentu vytvořte cluster s uzlem jeden pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-105">The steps in this document create a cluster with one worker node.</span></span> <span data-ttu-id="8a9f5-106">Pokud máte v plánu na víc než 32 uzlů pracovního procesu, při vytváření clusteru nebo škálování clusteru po vytvoření, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14GB paměti ram.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-106">If you plan on more than 32 worker nodes, either at cluster creation or by scaling the cluster after creation, you need to select a head node size with at least 8 cores and 14GB ram.</span></span>
>
> <span data-ttu-id="8a9f5-107">Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="8a9f5-107">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a9f5-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8a9f5-108">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="8a9f5-109">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-109">**An Azure subscription**.</span></span> <span data-ttu-id="8a9f5-110">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="8a9f5-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="8a9f5-111">**Účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-111">**An Azure storage account**.</span></span> <span data-ttu-id="8a9f5-112">V tématu [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="8a9f5-112">See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>
* <span data-ttu-id="8a9f5-113">**Visual Studio 2013, Visual Studio 2015 nebo Visual Studio 2017**.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-113">**Visual Studio 2013, Visual Studio 2015 or Visual Studio 2017**.</span></span>

## <a name="create-clusters"></a><span data-ttu-id="8a9f5-114">Vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="8a9f5-114">Create clusters</span></span>

1. <span data-ttu-id="8a9f5-115">Otevřete Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-115">Open Visual Studio 2017.</span></span>
2. <span data-ttu-id="8a9f5-116">Vytvořte novou aplikaci konzoly Visual C#.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-116">Create a new Visual C# console application.</span></span>
3. <span data-ttu-id="8a9f5-117">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet**a potom klikněte na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-117">From the **Tools** menu, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="8a9f5-118">V konzole pro instalaci balíčků, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8a9f5-118">Run the following command in the console to install the packages:</span></span>

    ```powershell
    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight
    ```

    <span data-ttu-id="8a9f5-119">Tyto příkazy přidat knihovny .NET a odkazy na ně pro aktuální projekt sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-119">These commands add .NET libraries and references to them to the current Visual Studio project.</span></span>
5. <span data-ttu-id="8a9f5-120">V Průzkumníku řešení poklikejte na **Program.cs** otevřete ho vložte následující kód a zadejte hodnoty pro proměnné:</span><span class="sxs-lookup"><span data-stu-id="8a9f5-120">From Solution Explorer, double-click **Program.cs** to open it, paste the following code, and provide values for the variables:</span></span>

    ```csharp
    using System;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    namespace CreateHDInsightCluster
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;

            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            private const string ExistingResourceGroupName = "<Enter Resource Group Name>";
            private const string ExistingStorageName = "<Enter Default Storage Account Name>.blob.core.windows.net";
            private const string ExistingStorageKey = "<Enter Default Storage Account Key>";
            private const string ExistingBlobContainer = "<Enter Default Bob Container Name>";

            private const string NewClusterName = "<Enter HDInsight Cluster Name>";
            private const int NewClusterNumNodes = 2;
            private const string NewClusterLocation = "EAST US 2";     // Must be the same as the default Storage account
            private const OSType NewClusterOSType = OSType.Linux;
            private const string NewClusterType = "Hadoop";
            private const string NewClusterVersion = "3.5";
            private const string NewClusterUsername = "admin";
            private const string NewClusterPassword = "<Enter HTTP User Password>";
            private const string NewClusterSshUserName = "sshuser";

            // You can use eitehr password or public key. See https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix
            private const string NewClusterSshPassword = "<Enter SSH User Password>";
            private const string NewClusterSshPublicKey = @"---- BEGIN SSH2 PUBLIC KEY ----
                Comment: ""rsa-key-20150731""
                AAAAB3NzaC1yc2EAAAABJQAAAQEA4QiCRLqT7fnmUA5OhYWZNlZo6lLaY1c+IRsp
                gmPCsJVGQLu6O1wqcxRqiKk7keYq8bP5s30v6bIljsLZYTnyReNUa5LtFw7eauGr
                yVt3Pve6ejfWELhbVpi0iq8uJNFA9VvRkz8IP1JmjC5jsdnJhzQZtgkIrdn3w0e6
                WVfu15kKyY8YAiynVbdV51EB0SZaSLdMZkZQ81xi4DDtCZD7qvdtWEFwLa+EHdkd
                pzO36Mtev5XvseLQqzXzZ6aVBdlXoppGHXkoGHAMNOtEWRXpAUtEccjpATsaZhQR
                zZdZlzHduhM10ofS4YOYBADt9JohporbQVHM5w6qUhIgyiPo7w==
                ---- END SSH2 PUBLIC KEY ----"; //replace the public key with your own

            static void Main(string[] args)
            {
                System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

                // Authenticate and get a token
                var authToken = GetTokenCloudCredentials(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // Set parameters for the new cluster
                var parameters = new ClusterCreateParameters
                {
                    ClusterSizeInNodes = NewClusterNumNodes,
                    UserName = NewClusterUsername,
                    ClusterType = NewClusterType,
                    OSType = NewClusterOSType,
                    Version = NewClusterVersion,

                    // Use an Azure storage account as the default storage
                    DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingBlobContainer),

                    // Is the cluster type RServer? If so, you can set the EdgeNodeSize.
                    // Otherwise, the default VM size is used.
                    //EdgeNodeSize = "Standard_D12_v2",

                    Password = NewClusterPassword,
                    Location = NewClusterLocation,

                    SshUserName = NewClusterSshUserName,
                    SshPassword = NewClusterSshPassword,
                    //SshPublicKey = NewClusterSshPublicKey
                };

                // Is the cluster type RServer? If so, add the RStudio configuration option.
                /*
                parameters.Configurations.Add(
                    "rserver",
                    new Dictionary<string, string>()
                    {
                        { "rstudio", "true" }
                    }
                );
                */

                // Create the cluster
                _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

                System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate to an Azure subscription and retrieve an authentication token
            /// </summary>
            static TokenCloudCredentials GetTokenCloudCredentials(string TenantId, string ClientId, string SubscriptionId)
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
    ```

6. <span data-ttu-id="8a9f5-121">Nahraďte hodnoty členů třídy.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-121">Replace the class member values.</span></span>
7. <span data-ttu-id="8a9f5-122">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-122">Press **F5** to run the application.</span></span> <span data-ttu-id="8a9f5-123">Okno konzoly by měla otevřít a zobrazit stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-123">A console window should open and display the status of the application.</span></span> <span data-ttu-id="8a9f5-124">Zobrazí se výzva k zadání přihlašovacích údajů účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-124">You are prompted to enter your Azure account credentials.</span></span> <span data-ttu-id="8a9f5-125">Ho může trvat několik minut pro vytvoření clusteru HDInsight, obvykle přibližně 15.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-125">It can take several minutes to create an HDInsight cluster, normally around 15.</span></span>

## <a name="use-bootstrap"></a><span data-ttu-id="8a9f5-126">Použití bootstrap</span><span class="sxs-lookup"><span data-stu-id="8a9f5-126">Use bootstrap</span></span>

<span data-ttu-id="8a9f5-127">Pomocí bootstrap, můžete nakonfigurovat nastavení přidání během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-127">Using bootstrap, you can configure addition settings during the cluster creations.</span></span>  <span data-ttu-id="8a9f5-128">Další informace najdete v tématu [clusterů HDInsight přizpůsobit pomocí Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).</span><span class="sxs-lookup"><span data-stu-id="8a9f5-128">For more information, see [Customize HDInsight clusters using Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).</span></span>

<span data-ttu-id="8a9f5-129">Upravte ukázkový v [vytvořit clustery se](#create-clusters) konfigurace nastavení Hive:</span><span class="sxs-lookup"><span data-stu-id="8a9f5-129">Modify the sample in [Create clusters](#create-clusters) to configure a Hive setting:</span></span>

```csharp
static void Main(string[] args)
{
    System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

    // Authenticate and get a token
    var authToken = GetTokenCloudCredentials(TenantId, ClientId, SubscriptionId);
    // Flag subscription for HDInsight, if it isn't already.
    EnableHDInsight(authToken);
    // Get an HDInsight management client
    _hdiManagementClient = new HDInsightManagementClient(authToken);

    // Set parameters for the new cluster
    var extendedParameters = new ClusterCreateParametersExtended
    {
        Location = NewClusterLocation,
        Properties = new ClusterCreateProperties
        {
            ClusterDefinition = new ClusterDefinition
            {
                ClusterType = NewClusterType.ToString()
            },
            ClusterVersion = NewClusterVersion,
            OperatingSystemType = NewClusterOSType
        }
    };

    var coreConfigs = new Dictionary<string, string>
    {
        {"fs.defaultFS", string.Format("wasb://{0}@{1}", ExistingBlobContainer, ExistingStorageName)},
        {
            string.Format("fs.azure.account.key.{0}", ExistingStorageName),
            ExistingStorageKey
        }
    };

    // bootstrap
    var hiveConfigs = new Dictionary<string, string>
    {
        { "hive.metastore.client.socket.timeout", "90"}
    };

    var gatewayConfigs = new Dictionary<string, string>
    {
        {"restAuthCredential.isEnabled", "true"},
        {"restAuthCredential.username", NewClusterUsername},
        {"restAuthCredential.password", NewClusterPassword}
    };

    var configurations = new Dictionary<string, Dictionary<string, string>>
    {
        {"core-site", coreConfigs},
        {"gateway", gatewayConfigs},
        {"hive-site", hiveConfigs}
    };

    var serializedConfig = JsonConvert.SerializeObject(configurations);
    extendedParameters.Properties.ClusterDefinition.Configurations = serializedConfig;

    var sshPublicKeys = new List<SshPublicKey>();
    var sshPublicKey = new SshPublicKey
    {
        CertificateData =
            string.Format("ssh-rsa {0}", NewClusterSshPublicKey)
    };
    sshPublicKeys.Add(sshPublicKey);

    var headNode = new Role
    {
        Name = "headnode",
        TargetInstanceCount = 2,
        HardwareProfile = new HardwareProfile
        {
            VmSize = "Large"
        },
        OsProfile = new OsProfile
        {
            LinuxOperatingSystemProfile = new LinuxOperatingSystemProfile
            {
                UserName = NewClusterSshUserName,
                Password = NewClusterSshPassword //,
                // When use a SSH pulbic key, make sure to remove comments, headers and trailers, and concatenate the key into one line 
                //SshProfile = new SshProfile
                //{
                //    SshPublicKeys = sshPublicKeys
                //}
            }
        }
    };

    var workerNode = new Role
    {
        Name = "workernode",
        TargetInstanceCount = NewClusterNumNodes,
        HardwareProfile = new HardwareProfile
        {
            VmSize = "Large"
        },
        OsProfile = new OsProfile
        {
            LinuxOperatingSystemProfile = new LinuxOperatingSystemProfile
            {
                UserName = NewClusterSshUserName,
                Password = NewClusterSshPassword //,
                //SshProfile = new SshProfile
                //{
                //    SshPublicKeys = sshPublicKeys
                //}
            }
        }
    };

    extendedParameters.Properties.ComputeProfile = new ComputeProfile();
    extendedParameters.Properties.ComputeProfile.Roles.Add(headNode);
    extendedParameters.Properties.ComputeProfile.Roles.Add(workerNode);

    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, extendedParameters);

    System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
    System.Console.ReadLine();
}
```

## <a name="use-script-action"></a><span data-ttu-id="8a9f5-130">Použití akce skriptu</span><span class="sxs-lookup"><span data-stu-id="8a9f5-130">Use Script Action</span></span>

<span data-ttu-id="8a9f5-131">Pomocí akce skriptu, můžete nakonfigurovat další nastavení při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-131">Using Script Action, you can configure additional settings during cluster creations.</span></span>  <span data-ttu-id="8a9f5-132">Další informace najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8a9f5-132">For more information, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="8a9f5-133">Upravte ukázkový v [vytvořit clustery se](#create-clusters) volání akce skriptu k instalaci R:</span><span class="sxs-lookup"><span data-stu-id="8a9f5-133">Modify the sample in [Create clusters](#create-clusters) to call a Script Action to install R:</span></span>

```csharp
static void Main(string[] args)
{
    System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

    // Authenticate and get a token
    var authToken = GetTokenCloudCredentials(TenantId, ClientId, SubscriptionId);
    // Flag subscription for HDInsight, if it isn't already.
    EnableHDInsight(authToken);
    // Get an HDInsight management client
    _hdiManagementClient = new HDInsightManagementClient(authToken);

    // Set parameters for the new cluster
    var parameters = new ClusterCreateParameters
    {
        ClusterSizeInNodes = NewClusterNumNodes,
        Location = NewClusterLocation,
        ClusterType = NewClusterType,
        OSType = NewClusterOSType,
        Version = NewClusterVersion,

        DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingBlobContainer),

        UserName = NewClusterUsername,
        Password = NewClusterPassword,
        SshUserName = NewClusterSshUserName,
        SshPublicKey = NewClusterSshPublicKey
    };

    ScriptAction rScriptAction = new ScriptAction("Install R",
        new Uri("https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh"), "");

    parameters.ScriptActions.Add(ClusterNodeType.HeadNode,new System.Collections.Generic.List<ScriptAction> { rScriptAction});
    parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { rScriptAction });

    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

    System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
    System.Console.ReadLine();
}
```

## <a name="troubleshoot"></a><span data-ttu-id="8a9f5-134">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="8a9f5-134">Troubleshoot</span></span>

<span data-ttu-id="8a9f5-135">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="8a9f5-135">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a9f5-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a9f5-136">Next steps</span></span>
<span data-ttu-id="8a9f5-137">Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující informace o práci s vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="8a9f5-137">Now that you have successfully created an HDInsight cluster, use the following to learn how to work with your cluster.</span></span> 

### <a name="hadoop-clusters"></a><span data-ttu-id="8a9f5-138">Clustery Hadoop</span><span class="sxs-lookup"><span data-stu-id="8a9f5-138">Hadoop clusters</span></span>
* [<span data-ttu-id="8a9f5-139">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-139">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="8a9f5-140">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-140">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="8a9f5-141">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-141">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="8a9f5-142">Clustery HBase</span><span class="sxs-lookup"><span data-stu-id="8a9f5-142">HBase clusters</span></span>
* [<span data-ttu-id="8a9f5-143">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-143">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="8a9f5-144">Vývoj aplikací v jazyce Java pro HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-144">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="8a9f5-145">Clustery Storm</span><span class="sxs-lookup"><span data-stu-id="8a9f5-145">Storm clusters</span></span>
* [<span data-ttu-id="8a9f5-146">Vývoj topologie Java pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-146">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="8a9f5-147">Použití komponent, Python v Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-147">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="8a9f5-148">Nasazení a monitorování topologie se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-148">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a><span data-ttu-id="8a9f5-149">Clustery Spark</span><span class="sxs-lookup"><span data-stu-id="8a9f5-149">Spark clusters</span></span>
* [<span data-ttu-id="8a9f5-150">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="8a9f5-150">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="8a9f5-151">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="8a9f5-151">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="8a9f5-152">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="8a9f5-152">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="8a9f5-153">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="8a9f5-153">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="8a9f5-154">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="8a9f5-154">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="run-jobs"></a><span data-ttu-id="8a9f5-155">Spouštění úloh</span><span class="sxs-lookup"><span data-stu-id="8a9f5-155">Run jobs</span></span>
* [<span data-ttu-id="8a9f5-156">Spouštět úlohy Hive v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8a9f5-156">Run Hive jobs in HDInsight using .NET SDK</span></span>](hdinsight-hadoop-use-hive-dotnet-sdk.md)
* [<span data-ttu-id="8a9f5-157">Spuštění úlohy Pig v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8a9f5-157">Run Pig jobs in HDInsight using .NET SDK</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md)
* [<span data-ttu-id="8a9f5-158">Spuštění úloh Sqoop v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8a9f5-158">Run Sqoop jobs in HDInsight using .NET SDK</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)
* [<span data-ttu-id="8a9f5-159">Spuštění úloh Oozie v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8a9f5-159">Run Oozie jobs in HDInsight</span></span>](hdinsight-use-oozie.md)

