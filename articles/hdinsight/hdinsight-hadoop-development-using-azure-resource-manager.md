---
title: "aaaMigrate tooAzure Resource Manager nástroje pro HDInsight | Microsoft Docs"
description: "Jak nástroje toomigrate tooAzure vývoj Resource Manageru pro clustery HDInsight"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: c087ae63d2544e5badae6be9c258f783aa92e2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Migrace tooAzure Resource Manager vývojové nástroje založené pro clustery služby HDInsight

HDInsight je místo na základě Azure Service Manager ASM nástroje začne pro HDInsight. Pokud používáte prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo hello SDK rozhraní .NET HDInsight toowork s clustery HDInsight, jste podporovali toouse hello založené na Azure Resource Manager ARM verze prostředí PowerShell, rozhraní příkazového řádku a .NET SDK do budoucna. Tento článek obsahuje ukazatele toomigrate toohello nový založené na ARM přístup. Pokud je to možné, v tomto článku také upozornění na hello rozdíly mezi hello ASM a ARM přístupy pro HDInsight.

> [!IMPORTANT]
> Podpora Hello ASM na základě prostředí PowerShell, rozhraní příkazového řádku, a .NET SDK se ukončí na **1. ledna 2017**.
> 
> 

## <a name="migrating-azure-cli-tooazure-resource-manager"></a>Migrace tooAzure rozhraní příkazového řádku Azure Resource Manager
Hello příkazového řádku Azure CLI nyní výchozí tooAzure režimu Resource Manager (ARM), pokud provádíte upgrade z předchozí instalace; v takovém případě musíte toouse hello `azure config mode arm` příkaz tooswitch tooARM režimu.

základní příkazy Hello této hello rozhraní příkazového řádku Azure poskytuje toowork s HDInsight pomocí Azure Service Management (ASM) jsou hello stejné při použití ARM; ale některé parametry a přepínače může mít nové názvy a existuje mnoho nových parametrů při použití ARM. Například můžete nyní použít `azure hdinsight cluster create` toospecify hello virtuální sítě Azure, který by měl být vytvořen v clusteru, nebo Hive a informace metaúložiště Oozie.

Základní příkazy pro práci s HDInsight prostřednictvím Správce Azure Resource Manager jsou:

* `azure hdinsight cluster create`-Vytvoří nový cluster HDInsight
* `azure hdinsight cluster delete`-Odstraní stávající cluster HDInsight
* `azure hdinsight cluster show`– Zobrazí informace o stávajícího clusteru
* `azure hdinsight cluster list`-obsahuje seznam clustery HDInsight pro vaše předplatné Azure

Použití hello `-h` přepínač tooinspect hello parametry a přepínače, které jsou k dispozici u každého příkazu.

### <a name="new-commands"></a>Nové příkazy
Nové příkazy, které jsou k dispozici s Azure Resource Manager jsou:

* `azure hdinsight cluster resize`-dynamicky změny hello počet uzlů pracovního procesu v clusteru hello
* `azure hdinsight cluster enable-http-access`– umožňuje clusteru toohello přístup HTTPs (na ve výchozím nastavení)
* `azure hdinsight cluster disable-http-access`– Zakáže clusteru toohello přístup HTTPs
* `azure hdinsight script-action`-obsahuje příkazy pro vytváření nebo správu akcí skriptů v clusteru
* `azure hdinsight config`-poskytuje příkazů pro vytvoření konfiguračního souboru, který lze použít s hello `hdinsight cluster create` příkaz tooprovide informace o konfiguraci.

### <a name="deprecated-commands"></a>Nepoužívané příkazy
Pokud používáte hello `azure hdinsight job` příkazy toosubmit úlohy tooyour HDInsight clusteru, tyto nejsou k dispozici prostřednictvím hello příkazy ARM. Pokud potřebujete tooprogrammatically odeslání úlohy tooHDInsight z skriptů, měli byste místo toho použít hello rozhraní REST API poskytovaných v HDInsight. Další informace o odesílání úloh pomocí rozhraní REST API najdete v tématu hello následující dokumenty.

* [Spuštění úloh MapReduce s Hadoop v HDInsight pomocí cURL](hdinsight-hadoop-use-mapreduce-curl.md)
* [Spouštění dotazů Hive se systémem Hadoop v HDInsight pomocí cURL](hdinsight-hadoop-use-hive-curl.md)
* [Spuštění úlohy Pig s Hadoop v HDInsight pomocí cURL](hdinsight-hadoop-use-pig-curl.md)

Informace o jiných způsobech toorun MapReduce, Hive a vepřových interaktivně, najdete v části [používání MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md), [používání Hive s Hadoop v HDInsight](hdinsight-use-hive.md), a [použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md).

### <a name="examples"></a>Příklady
**Vytvoření clusteru**

* Příkaz staré (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Nový příkaz (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Odstranění clusteru**

* Příkaz staré (ASM)-`azure hdinsight cluster delete myhdicluster`
* Nový příkaz (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`

**Seznam clustery**

* Příkaz staré (ASM)-`azure hdinsight cluster list`
* Nový příkaz (ARM)-`azure hdinsight cluster list`

> [!NOTE]
> Pro příkaz seznamu hello, zadání hello skupinu prostředků pomocí `-g` vrátí pouze clustery hello v hello zadaná skupina prostředků.
> 
> 

**Zobrazit informace o clusteru**

* Příkaz staré (ASM)-`azure hdinsight cluster show myhdicluster`
* Nový příkaz (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-tooazure-resource-manager"></a>Migrace tooAzure prostředí PowerShell Azure Resource Manager
Hello obecné informace o prostředí Azure PowerShell v režimu Azure Resource Manager (ARM) hello lze najít na [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).

Hello rutiny Azure PowerShell ARM může být nainstalovaná-souběžného s hello ASM rutiny. rutiny Hello ze dvou režimů hello dají rozlišovat podle jejich názvy.  má režimu ARM Hello *AzureRmHDInsight* v názvech rutiny hello porovnávání příliš*AzureHDInsight* v režimu ASM hello.  Například *New-AzureRmHDInsightCluster* vs. *Nové AzureHDInsightCluster*. Názvy zprávy mohou mít parametry a přepínače, existuje mnoho nových parametrů k dispozici při použití a ARM.  Například několik rutin vyžadovat nového přepínače názvem *- ResourceGroupName*. 

Před použitím rutin HDInsight hello, musíte připojit tooyour účet Azure a vytvořit novou skupinu prostředků:

* Login-AzureRmAccount nebo [vyberte AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). V tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Nový AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Přejmenovat rutiny
toolist hello HDInsight ASM rutiny v konzole Windows PowerShell:

    help *azurermhdinsight*

Hello následující tabulka uvádí hello ASM rutiny a jejich názvy v režimu ARM hello:

| Rutiny ASM | Rutiny ARM |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Přidat AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[Přidat AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Přidat AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[Přidat AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[Udělení AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[Udělení AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Vyvolání AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[Nové AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) |
| New-AzureHDInsightClusterConfig |[Nové AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[Nové AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[Nové AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[Nové AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[Nové AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[Nové AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Odebrat AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[AzureRmHDInsightHttpServicesAccess odvolání.](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[AzureRmHDInsightRdpServicesAccess odvolání.](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Počáteční AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[Stop-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Použití AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Počkejte AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Nové rutiny
Hello následují hello nové rutiny, které jsou dostupné jenom v režimu ARM hello. 

**Akce skriptu související rutiny:**

* **Get-AzureRmHDInsightPersistedScriptAction**: hello získá trvalé akce skriptu pro cluster s podporou a zobrazí je v chronologickém pořadí nebo získá podrobnosti pro akci zadaný trvalého skriptu. 
* **Get-AzureRmHDInsightScriptActionHistory**: získá hello v historii akcí skriptu pro cluster a seznamů v obráceném chronologickém pořadí nebo získá podrobnosti akce dříve spuštění skriptu. 
* **Odebrat AzureRmHDInsightPersistedScriptAction**: Odebere akcí trvalého skriptu z clusteru služby HDInsight.
* **Set-AzureRmHDInsightPersistedScriptAction**: Nastaví dříve spustit skript akce toobe akcí trvalého skriptu.
* **Odeslání AzureRmHDInsightScriptAction**: odešle nový cluster Azure HDInsight akce tooan skriptu. 

Využití Další informace najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter identity související rutiny:**

* **Přidat AzureRmHDInsightClusterIdentity**: Přidá objekt konfigurace clusteru tooa clusteru identity tak, aby hello clusteru HDInsight můžete přístup k Azure Data Lake úložiště. V tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Příklady
**Vytvoření clusteru**

Příkaz staré (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Nový příkaz (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


**Odstranění clusteru**

Příkaz staré (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Nový příkaz (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Seznam clusteru**

Příkaz staré (ASM):

    Get-AzureHDInsightCluster

Nový příkaz (ARM):

    Get-AzureRmHDInsightCluster 

**Zobrazit clusteru**

Příkaz staré (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Nový příkaz (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Další ukázky
* [Vytvoření clusterů HDInsight](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Odeslání úloh Hive](hdinsight-hadoop-use-hive-powershell.md)
* [Odeslání úlohy Pig](hdinsight-hadoop-use-pig-powershell.md)
* [Odesílání úloh Sqoop](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-toohello-arm-based-hdinsight-net-sdk"></a>Migrace toohello založené na ARM HDInsight .NET SDK
Dobrý den, na základě Azure Service Management [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) je nyní zastaralý. Jsou podporovali toouse hello založený na správě prostředků Azure [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx). Hello následující HDInsight se systémem ASM balíčky jsou zastaralá.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Tato část obsahuje ukazatele toomore informace o tom, tooperform určité úlohy pomocí hello založené na ARM SDK.

| Postup... pomocí hello SDK HDInsight založené na ARM | Odkazy |
| --- | --- |
| Vytvoření clusterů HDInsight pomocí sady .NET SDK |V tématu [Tvorba clusterů HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| Přizpůsobení clusteru pomocí akce skriptu pomocí .NET SDK |V tématu [HDInsight Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Ověření aplikací interaktivně pomocí .NET SDK služby Azure Active Directory |V tématu [spouštění dotazů Hive pomocí sady .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md). Hello fragment kódu v tomto článku používá hello interaktivního ověřování přístup. |
| Ověření aplikací interaktivně pomocí .NET SDK služby Azure Active Directory |V tématu [vytvořit neinteraktivní aplikace pro HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Odeslání úlohy Hive pomocí sady .NET SDK |V tématu [úlohy odeslání Hive](hdinsight-hadoop-use-hive-dotnet-sdk.md) |
| Odeslat úlohu Pig pomocí sady .NET SDK |V tématu [úlohy odeslání Pig](hdinsight-hadoop-use-pig-dotnet-sdk.md) |
| Odeslání úlohy Sqoop pomocí sady .NET SDK |V tématu [Sqoop odeslání úlohy](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |
| Seznam clusterů HDInsight pomocí sady .NET SDK |V tématu [clusterů HDInsight seznamu](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Škálování clusterů HDInsight pomocí sady .NET SDK |V tématu [clusterů HDInsight škálování](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Udělení nebo odvolání přístupu tooHDInsight clustery pomocí sady .NET SDK |V tématu [udělení nebo odvolání přístupu tooHDInsight clustery](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| Aktualizovat pověření uživatele HTTP pro clustery služby HDInsight pomocí sady .NET SDK |V tématu [přihlašovací údaje uživatele HTTP aktualizace pro clustery služby HDInsight](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Najít hello výchozí účet úložiště pro clustery služby HDInsight pomocí sady .NET SDK |V tématu [najít hello výchozí účet úložiště pro clustery služby HDInsight](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| Odstranění clusterů HDInsight pomocí sady .NET SDK |V tématu [clusterů HDInsight odstranit](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Příklady
Tady jsou některé příklady na to, jak je operace používá hello na základě ASM SDK a fragmentu kódu ekvivalentní hello hello založené na ARM SDK.

**Vytvoření klienta CRUD clusteru**

* Příkaz staré (ASM)
  
        //Certificate auth
        //This logs hello application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Nový příkaz (ARM) (hlavní autorizace Service)
  
        //Service principal auth
        //This will log hello application in as itself, rather than on behalf of a specific user.
        //For details, including how tooset up hello application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Nový příkaz (ARM) (autorizace uživatelů)
  
        //User auth
        //This will log hello application in on behalf of hello user.
        //hello end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Vytvoření clusteru**

* Příkaz staré (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Nový příkaz (ARM)
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**Povolíte přístup protokolu HTTP**

* Příkaz staré (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Nový příkaz (ARM)
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Odstranění clusteru**

* Příkaz staré (ASM)
  
        client.DeleteCluster(dnsName);
* Nový příkaz (ARM)
  
        client.Clusters.Delete(resourceGroup, dnsname);

