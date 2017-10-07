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
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Správa clusterů systému Hadoop v HDInsight pomocí sady .NET SDK
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Zjistěte, jak toomanage HDInsight clusterů pomocí [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).

**Požadavky**

Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="connect-tooazure-hdinsight"></a>Připojit tooAzure HDInsight

Je třeba hello následující balíčky Nuget:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

Hello následující příklad kódu ukazuje, jak tooconnect tooAzure než budete moct spravovat HDInsight clustery v rámci vašeho předplatného Azure.

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

Zobrazí se výzva a při spuštění tohoto programu.  Pokud nechcete, aby toosee hello řádku, přečtěte si téma [vytvořit neinteraktivního ověřování aplikace .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

## <a name="create-clusters"></a>Vytváření clusterů
V tématu [hello vytvořit systémem Linux clusterů v HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

## <a name="list-clusters"></a>Seznam clustery
Hello následující fragment kódu obsahuje clustery a některé vlastnosti:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a>Odstranění clusterů
Použijte následující toodelete fragmentu kódu cluster synchronně nebo asynchronně hello: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a>Škálování clusterů
Hello clusteru škálování funkce vám umožní toochange hello počet uzlů pracovního procesu používá cluster, který běží v Azure HDInsight bez nutnosti toore – vytvoření clusteru hello.

> [!NOTE]
> Pouze clustery s HDInsight verze 3.1.3 nebo vyšší nejsou podporovány. Pokud si nejste jistí hello verze vašeho clusteru, můžete zkontrolovat hello stránku vlastností.  V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
> 
> 

Hello dopad změny hello počet uzlů dat pro každý typ clusteru podporuje HDInsight:

* Hadoop
  
    Můžete zvýšit bezproblémově hello počet uzlů pracovního procesu v clusteru Hadoop, který běží bez dopadu na všechny úlohy čekající na vyřízení nebo spuštěné. Nové úlohy můžete také odeslány, když probíhá operace hello. Selhání v rámci operace škálování pohodlné zpracování tak, aby hello clusteru vždy zůstává ve funkčním stavu.
  
    Pokud se Hadoop cluster měřítko snížením hello počet uzlů data, některé služby hello v clusteru hello restartovat. To způsobí, že všechny spuštěné a čeká se na úlohy toofail na dokončení hello hello operace škálování. Po dokončení operace hello může, ale odešlete znovu hello úlohy.
* HBase
  
    Můžete bezproblémově přidat nebo odebrat uzly clusteru HBase tooyour, když je spuštěná. Místní servery jsou automaticky vyváženy během několika minut od hello operace škálování. Po přihlášení do hello headnode clusteru a spuštěné hello následujících příkazů z okna příkazového řádku však můžete také ručně vyvážit hello místní servery:
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* Storm
  
    Můžete bez problémů přidat nebo odebrat cluster Storm tooyour uzly dat je spuštěna. Ale po úspěšném dokončení hello operace škálování, budete potřebovat toorebalance hello topologie.
  
    Vyrovnává lze dosáhnout dvěma způsoby:
  
  * Storm webového uživatelského rozhraní
  * Nástroj pro rozhraní příkazového řádku (CLI)
    
    Podrobnosti najdete toohello [dokumentaci Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) další podrobnosti.
    
    Hello Storm webového uživatelského rozhraní není k dispozici v clusteru HDInsight hello:
    
    ![Obnovte rovnováhu škálování Storm v HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    Tady je příklad jak toouse hello rozhraní příkazového řádku příkaz topologie Storm toorebalance hello:
    
        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Následující kód fragment kódu ukazuje, jak Hello tooresize cluster synchronně nebo asynchronně:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a>Udělení nebo odvolání přístupu
Clustery HDInsight mít hello následující HTTP webové služby (všechny tyto služby mají RESTful koncových bodů):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Ve výchozím nastavení jsou tyto služby oprávnění pro přístup. Můžete můžete odvolat nebo udělit přístup hello. toorevoke:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

toogrant:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> Pomocí udělení nebo odvolání přístupu hello, obnoví hello clusteru uživatelské jméno a heslo.
> 
> 

To lze provést také prostřednictvím hello portálu. V tématu [hello spravovat HDInsight pomocí portálu Azure][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>Aktualizovat pověření uživatele HTTP
Je hello stejný postup jako [HTTP udělení nebo odvolání přístupu](#grant/revoke-access). Pokud hello clusteru byla udělena hello přístup protokolu HTTP, musí se nejdřív odvolat.  A pak udělují přístup hello s novými pověřeními uživatele HTTP.

## <a name="find-hello-default-storage-account"></a>Najít hello výchozí účet úložiště
Hello následující fragment kódu ukazuje, jak tooget hello výchozí název účtu úložiště a hello výchozí klíč účtu úložiště pro cluster.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a>Odesílání úloh
**úlohy MapReduce toosubmit**

V tématu [ukázky spustit Hadoop MapReduce v HDInsight](hdinsight-hadoop-run-samples-linux.md).

**toosubmit úloh Hive** 

V tématu [spouštění dotazů Hive pomocí sady .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**úlohy Pig toosubmit**

V tématu [úlohy spustit Pig pomocí sady .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**toosubmit Sqoop úlohy**

V tématu [použití nástroje Sqoop se HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).

**toosubmit Oozie úlohy**

V tématu [Oozie použití s Hadoop toodefine a spouštění pracovních postupů v prostředí HDInsight](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-tooazure-blob-storage"></a>Nahrání dat tooAzure Blob storage
V tématu [nahrát data tooHDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Viz také
* [HDInsight .NET SDK referenční dokumentaci k nástroji](https://msdn.microsoft.com/library/mt271028.aspx)
* [Spravovat HDInsight pomocí hello portálu Azure][hdinsight-admin-portal]
* [Spravovat HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]
* [Vytvoření clusterů HDInsight][hdinsight-provision]
* [Nahrání dat tooHDInsight][hdinsight-upload-data]
* [Začínáme se službou Azure HDInsight][hdinsight-get-started]

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


