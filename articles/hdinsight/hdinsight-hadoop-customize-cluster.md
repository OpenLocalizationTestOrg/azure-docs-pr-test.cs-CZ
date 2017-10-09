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
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Přizpůsobení clusterů HDInsight se systémem Windows pomocí akce skriptu
**Skript akce** lze použít tooinvoke [vlastní skripty](hdinsight-hadoop-script-actions.md) během procesu vytváření clusteru hello k instalaci na clusteru s podporou další software.

Hello informace v tomto článku je konkrétní na základě tooWindows clusterů HDInsight. V clusterech se systémem Linux naleznete v části [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Clustery prostředí HDInsight lze přizpůsobit v různých další způsoby, například včetně další účty Azure Storage, změna hello Hadoop soubory konfigurace (core-site.xml, hive-site.xml atd.) nebo přidání sdílené knihovny (například Hive, Oozie) do společné umístění v clusteru hello. Toto vlastní nastavení můžete provést pomocí prostředí Azure PowerShell hello .NET SDK služby Azure HDInsight, nebo hello portálu Azure. Další informace najdete v tématu [vytvoření Hadoop clusterů v HDInsight][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-hello-cluster-creation-process"></a>Akce skriptu v procesu vytváření clusteru hello
Akce skriptu se používá pouze během hello proces vytváření clusteru. Hello následující diagram znázorňuje, pokud je během procesu vytváření hello provést akce skriptu:

![Přizpůsobení cluster HDInsight a fáze při vytváření clusteru][img-hdi-cluster-states]

Po spuštění skriptu hello hello cluster zadá hello **ClusterCustomization** fáze. V této fázi je spuštěna pod účtem správce systému hello hello skriptu, paralelně na všechny hello zadaný uzly v clusteru hello a poskytuje úplný správce oprávnění na uzlech hello.

> [!NOTE]
> Vzhledem k tomu, že máte oprávnění správce na uzlech clusteru hello během **ClusterCustomization** fáze, můžete použít hello skriptu tooperform operací, jako je spouštění a zastavování služeb, včetně služby související s Hadoop. Proto v rámci skriptu hello, musíte zajistit této služby hello Ambari a další služby související s Hadoop jsou spuštěny před hello skriptu dokončení spuštění. Tyto služby jsou potřeba toosuccessfully zjistit hello stav a stav hello clusteru při jeho vytvoření. Pokud změníte žádnou konfiguraci v clusteru, který má vliv na tyto služby, musíte použít hello pomocných funkcí, které jsou k dispozici. Další informace o pomocných funkcí najdete v tématu [vyvíjet akce skriptu skripty pro HDInsight][hdinsight-write-script].
>
>

Hello výstup a protokoly chyb hello hello skriptu jsou uloženy v hello výchozí účet úložiště, které jste zadali pro hello cluster. Hello protokoly se ukládají v tabulce s názvem hello **u < \cluster-name-fragment >< \time-stamp > setuplog**. Toto jsou agregovaná protokoly ze skriptu hello spouštět na všech uzlech hello (hlavního uzlu a pracovní uzly) v clusteru hello.

Každý cluster může přijmout víc akcí skriptů, které jsou vyvolány v hello pořadí, ve kterém jsou uvedené. Skript může spuštěna v hello hlavního uzlu, hello pracovní uzly nebo obojí.

HDInsight nabízí několik hello tooinstall skripty v clusterech HDInsight následující součásti:

| Name (Název) | Skript |
| --- | --- |
| **Nainstalujte Spark** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. V tématu [instalací a použitím clustery Spark v HDInsight][hdinsight-install-spark]. |
| **Nainstalujte jazyk R** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. V tématu [instalací a použitím R v clusterech HDInsight][hdinsight-install-r]. |
| **Nainstalujte Solr** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install.md). |
| - **Nainstalujte Giraph** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install.md). |
| **Předběžné načtení knihovny Hive** |https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. V tématu [přidat Hive knihovny v clusterech prostředí HDInsight](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-hello-azure-portal"></a>Volání skriptů pomocí hello portálu Azure
**Z hello portálu Azure**

1. Zahájení vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
2. V části volitelné konfigurace pro hello **akcí skriptů** okně klikněte na tlačítko **přidat akce skriptu** tooprovide podrobností o hello akce skriptu, jak je uvedeno níže:

    ![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "použití akce skriptu toocustomize clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Name (Název)</td>
            <td>Zadejte název akce skriptu hello.</td></tr>
        <tr><td>Identifikátor URI skriptu</td>
            <td>Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru. s</td></tr>
        <tr><td>HEAD/pracovního procesu</td>
            <td>Zadejte hello uzly (**Head** nebo **pracovní**) na úpravy hello je skript spuštěn.</b>.
        <tr><td>Parametry</td>
            <td>Zadejte parametry hello, pokud to vyžaduje hello skriptu.</td></tr>
    </table>

    Stiskněte klávesu ENTER tooadd více než jeden skript akce tooinstall více součástí, které v clusteru hello.
3. Klikněte na tlačítko **vyberte** toosave hello konfiguraci akce skriptu a pokračujte vytvoření clusteru.

## <a name="call-scripts-using-azure-powershell"></a>Volání skripty pomocí Azure PowerShell
Tento následující skript prostředí PowerShell ukazuje, jak tooinstall Spark v systému Windows na základě clusteru HDInsight.  

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


tooinstall další software, budete potřebovat soubor skriptu hello tooreplace ve skriptu hello:

Po zobrazení výzvy zadejte přihlašovací údaje hello hello clusteru. Může trvat několik minut, než se vytvoří hello clusteru.

## <a name="call-scripts-using-net-sdk"></a>Volání skripty pomocí sady .NET SDK
Hello následující příklad ukazuje, jak tooinstall Spark v systému Windows na základě clusteru HDInsight. tooinstall další software, budete potřebovat soubor skriptu hello tooreplace v kódu hello.

**toocreate clusteru HDInsight pomocí Spark**

1. Vytvořte konzolovou aplikaci C# v sadě Visual Studio.
2. Z konzoly Správce balíčků Nuget hello spusťte následující příkaz hello.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. Hello použijte následující příkazy using do souboru Program.cs hello:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. Umístěte hello kód ve třídě hello s hello následující:

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
5. Stiskněte klávesu **F5** toorun hello aplikace.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Podpora pro open-source softwaru použít na clustery HDInsight
Hello služby Microsoft Azure HDInsight je flexibilní platforma, která umožňuje aplikacím toobuild velkých objemů dat v cloudu hello pomocí prostředí technologie open source vytvořen kolem Hadoop. Microsoft Azure poskytuje určitou úroveň podpory pro technologie open source, jak je popsáno v hello **podporu oboru** části hello <a href="http://azure.microsoft.com/support/faq/" target="_blank">web Azure podporují – nejčastější dotazy</a>. Hello služba HDInsight poskytuje další úroveň podpory pro některé z hello součástí, jak je popsáno níže.

Existují dva typy open-source komponent, které jsou k dispozici v hello služby HDInsight:

* **Integrované komponenty** -tyto komponenty jsou předinstalované na clustery HDInsight a poskytují základní funkce služby hello clusteru. Například YARN ResourceManager, hello Hive dotazovací jazyk (HiveQL) a knihovna Mahout hello patří toothis kategorie. Úplný seznam součástí clusteru je k dispozici v [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight?](hdinsight-component-versioning.md) </a>.
* **Vlastní komponenty** -, jako uživatel hello clusteru, můžete nainstalovat nebo použít v vaše úlohy žádné součásti k dispozici v komunitě hello nebo vytvořené vámi.

Integrované komponenty jsou plně podporované, a bude pomoci tooisolate a vyřešit problémy související toothese součásti Microsoft Support.

> [!WARNING]
> Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporované a Microsoft Support bude pomoci tooisolate a vyřešit problémy související toothese součásti.
>
> Vlastní komponenty přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello. To může způsobit řešení problému hello nebo žádat, že jste tooengage dostupné kanály pro hello otevřít zdroj technologie, kterých se nachází hluboké znalosti pro tuto technologii. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).
>
>

Hello služba HDInsight poskytuje několik způsobů toouse vlastní součásti. Bez ohledu na to, jak součást použít nebo nainstalovat na clusteru hello hello stejnou úroveň podpory platí. Níže je seznam hello nejběžnější způsoby vlastní komponenty lze v clusterech HDInsight:

1. Úloha odeslání - Hadoop nebo jiné typy úloh, které spustit nebo používat vlastní komponenty může být odeslaná toohello clusteru.
2. Přizpůsobení clusteru – při vytváření clusteru, můžete zadat další nastavení a vlastní součásti, které se nainstalují na uzlech clusteru hello.
3. Ukázky - oblíbených vlastní součásti, Microsoft a ostatní mohou poskytnout ukázky použití těchto součástí v clusterech HDInsight hello. Tyto soubory jsou uvedeny bez podpory.

## <a name="develop-script-action-scripts"></a>Vývoj skriptů akce skriptu
V tématu [vyvíjet akce skriptu skripty pro HDInsight][hdinsight-write-script].

## <a name="see-also"></a>Viz také
* [Vytvoření clusterů systému Hadoop v HDInsight] [ hdinsight-provision-cluster] obsahuje pokyny, jak clusteru toocreate HDInsight pomocí jiné možnosti vlastního nastavení.
* [Vývoj skriptů akce skriptu pro HDInsight][hdinsight-write-script]
* [Nainstalovat a používat Spark v HDInsight clustery][hdinsight-install-spark]
* [Nainstalovat a používat R na clustery HDInsight][hdinsight-install-r]
* [Nainstalovat a používat Solr v clusterech HDInsight](hdinsight-hadoop-solr-install.md).
* [Nainstalovat a používat Giraph v clusterech HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fáze při vytváření clusteru"
