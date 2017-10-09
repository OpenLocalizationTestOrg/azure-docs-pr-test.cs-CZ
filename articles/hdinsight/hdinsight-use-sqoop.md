---
title: "aaaRun Apache Sqoop úlohy s Azure HDInsight (Hadoop) | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí Azure PowerShell z pracovní stanice toorun Sqoop import a export mezi clusteru Hadoop a Azure SQL database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bdac507704937d77921c9c13d70aa2434c7e3be4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a>Použití nástroje Sqoop se systémem Hadoop v HDInsight
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Zjistěte, jak toouse Sqoop v HDInsight tooimport a export mezi HDInsight cluster a Azure SQL database nebo databáze systému SQL Server.

I když Hadoop je přirozené volbou pro zpracování nestrukturovaných a částečně strukturovaných dat, jako jsou protokoly a soubory, může také existovat nutnost tooprocess strukturovaná data uložená v relačních databází.

[Sqoop] [ sqoop-user-guide-1.4.4] je tootransfer nástroj navržený dat mezi clusterů systému Hadoop a relačními databázemi. Můžete ji použít tooimport data ze systému správy relačních databází (RDBMS), jako je SQL Server, MySQL a Oracle do systému souborů Hadoop distributed hello (HDFS), transformovat hello data v Hadoop pomocí MapReduce nebo Hive a poté exportujte hello data zpět do RELAČNÍ. V tomto kurzu použijete databázi systému SQL Server pro relační databázi.

Sqoop verze, které jsou podporovány v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů hello poskytovaných v HDInsight?][hdinsight-versions]

## <a name="understand-hello-scenario"></a>Pochopení hello scénář

HDInsight cluster se dodává s ukázková data. Pomocí následujících dvou vzorcích hello:

* Soubor protokolu log4j, která se nachází v */example/data/sample.log*. Hello následující protokoly se extrahují z hello souboru:
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* Hive tabulku s názvem *hivesampletable*, který odkazuje na hello datový soubor nacházející se v */hive/warehouse/hivesampletable*. Hello tabulka obsahuje některé data mobilních zařízení. 
  
  | Pole | Datový typ |
  | --- | --- |
  | ClientID |Řetězec |
  | querytime |Řetězec |
  | trh |Řetězec |
  | deviceplatform |Řetězec |
  | devicemake |Řetězec |
  | devicemodel |Řetězec |
  | state |Řetězec |
  | Země |Řetězec |
  | querydwelltime |Double |
  | ID relace |bigint |
  | sessionpagevieworder |bigint |

Nejprve exportujete *sample.log* a *hivesampletable* toohello Azure SQL database nebo tooSQL serveru a pak import hello tabulku, která obsahuje data mobilních zařízení hello zálohování tooHDInsight pomocí hello následující cestu:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Vytvoření clusteru a databáze SQL
Tato část uvádí, jak toocreate cluster, databáze SQL a hello SQL databáze, schémata pro spuštěné hello kurz používání hello portál Azure a šablonu Azure Resource Manager. Hello šablony lze nalézt v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). šablony Resource Manageru Hello volá souboru bacpac balíček toodeploy hello tabulky schémata tooSQL databáze.  Hello souboru bacpac balíčku se nachází v kontejneru veřejného objektu blob, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Pokud chcete pro soubory souboru bacpac hello toouse kontejner privátní, použijte následující hodnoty v šabloně hello hello:
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

Pokud dáváte přednost toouse prostředí Azure PowerShell toocreate hello clusteru a hello SQL Database, najdete v části [příloha A](#appendix-a---a-powershell-sample).

1. Klikněte na tlačítko hello následující tooopen image šablony Resource Manageru v hello portálu Azure.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   

2. Zadejte hello následující vlastnosti:

    - **Předplatné**: Zadejte předplatné Azure.
    - **Skupina prostředků**: Vytvořte novou skupinu prostředků Azure, nebo vyberte existující skupinu prostředků.  Skupina prostředků je pro účely správy.  Je kontejner pro objekty.
    - **Umístění**: Vyberte oblast.
    - **Název clusteru**: Zadejte název pro hello Hadoop cluster.
    - **Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je admin.
    - **Uživatelské jméno a heslo SSH**.
    - **Databáze SQL serveru přihlašovací jméno a heslo**.
    - **_artifacts umístění**: používání hello výchozí hodnotu, pokud chcete, aby toouse backpac souboru v jiném umístění.
    - **_artifacts umístění Sas Token**: ponechat prázdné.
    - **Název souboru souboru Bacpac**: používání hello výchozí hodnotu, pokud chcete, aby toouse souboru backpac.
     
     Hello následující hodnoty jsou pevně kódovaný v části proměnných hello:
     
     | Výchozí název účtu úložiště | <CluterName>úložiště |
     | --- | --- |
     | Název serveru databáze SQL Azure |<ClusterName>DBServer |
     | Název databáze SQL Azure |<ClusterName>DB |
     
     Zapište tyto hodnoty.  Budete potřebovat později v kurzu hello.

3 klikněte na tlačítko **OK** toosave hello parametry.

4. z hello **vlastní nasazení** okně klikněte na tlačítko **skupiny prostředků** rozevírací pole a pak klikněte na **nový** toocreate novou skupinu prostředků. Hello skupina prostředků je kontejner, který seskupuje hello cluster, účet závislého úložiště hello a další propojené prostředky.

5. Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.

6. Klikněte na **Vytvořit**. Zobrazí se nová dlaždice s názvem odeslání nasazení pro šablonu nasazení. Trvá přibližně 20 minut toocreate hello clusteru a databáze SQL.

Pokud se rozhodnete toouse existující databázi Azure SQL nebo Microsoft SQL Server

* **Databáze SQL Azure**: musíte nakonfigurovat pravidlo brány firewall pro přístup k Azure SQL database serveru tooallow hello z pracovní stanice. Pokyny týkající se vytváření databáze Azure SQL a konfiguraci brány firewall hello najdete v tématu [začít používat Azure SQL database][sqldatabase-get-started]. 
  
  > [!NOTE]
  > Ve výchozím nastavení Azure SQL database umožňuje připojení z Azure služby, jako je Azure HDInsight. Pokud toto nastavení brány firewall je zakázáno, je třeba tooenable z hello portálu Azure. Pokyny týkající se vytváření databáze Azure SQL a konfigurace pravidel brány firewall, najdete v části [vytvořit a nakonfigurovat databázi SQL][sqldatabase-create-configue].
  > 
  > 
* **SQL Server**: Pokud je váš cluster HDInsight na hello stejné virtuální síti v Azure jako systém SQL Server, můžete použít kroky hello Tento článek tooimport a export dat tooa databáze SQL serveru.
  
  > [!NOTE]
  > HDInsight podporuje pouze na základě umístění virtuální sítě a aktuálně nefunguje s virtuálních sítích založených na skupinu vztahů.
  > 
  > 
  
  * toocreate a konfigurace virtuální sítě, najdete v části [vytvoření virtuální sítě pomocí portálu Azure hello](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    
    * Pokud používáte systém SQL Server ve vašem datovém centru, je nutné nakonfigurovat hello virtuální síti jako *site-to-site* nebo *point-to-site*.
      
      > [!NOTE]
      > Pro **point-to-site** virtuální sítě, SQL Server musí používat klienta VPN hello konfigurace aplikace, která je k dispozici z hello **řídicí panel** konfigurace virtuální sítě Azure.
      > 
      > 
    * Při použití systému SQL Server na virtuální počítač Azure, lze použít všechny konfigurace virtuální sítě, pokud hello virtuálního počítače, který je hostitelem SQL serveru je členem hello stejné virtuální síti jako HDInsight.
  * toocreate clusteru služby HDInsight ve virtuální síti, najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md)
    
    > [!NOTE]
    > SQL Server musíte také povolit ověřování. Musíte použít SQL Server hello toocomplete přihlášení kroky v tomto článku.
    > 
    > 

## <a name="run-sqoop-jobs"></a>Spuštění úloh Sqoop
HDInsight Sqoop úlohy můžete spustit pomocí různých metod. Pomocí hello následující toodecide tabulku, která metoda je pro vás nejvhodnější a potom postupujte podle hello odkaz návod.

| **Použít** Pokud chcete... | .. .an **interaktivní** prostředí | ... **batch** zpracování | .. při to **clusteru operačního systému** | .. .from to **klientský operační systém** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-use-sqoop-mac-linux.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X nebo systému Windows |
| [Sada .NET SDK pro Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |✔ |Linux nebo Windows |Windows (prozatím) |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |✔ |Linux nebo Windows |Windows |

## <a name="limitations"></a>Omezení
* Hromadně export - s Linuxovým systémem HDInsight, hello Sqoop konektor používaný tooexport data tooMicrosoft systému SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.
* Dávkování - s HDInsight se systémem Linux, při použití hello `-batch` přepnout při vložení, Sqoop provádí více vloží místo dávkování operace insert hello.

## <a name="next-steps"></a>Další kroky
Nyní jste se naučili, jak toouse Sqoop. toolearn více, najdete v části:

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použijte Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.
* [Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]: použití Hive letu tooanalyze zpoždění data a pak použijte Sqoop tooexport data tooan Azure SQL database.
* [Nahrání dat tooHDInsight][hdinsight-upload-data]: Najít další metody pro odesílání dat tooHDInsight/Azure Blob storage.

## <a name="appendix-a---a-powershell-sample"></a>Příloha A - ukázku prostředí PowerShell
Ukázkové prostředí PowerShell Hello provádí hello následující kroky:

1. Připojte tooAzure.
2. Vytvořte skupinu prostředků Azure. Další informace najdete v tématu [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md)
3. Vytvoření serveru Azure SQL Database, Azure SQL database a dvě tabulky. 
   
    Pokud místo toho používat SQL Server, použijte následující příkazy toocreate hello tabulky hello:
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    Hello nejjednodušší způsob, jak tooexamine hello databáze a tabulky je toouse Visual Studio. Hello databázového serveru a hello databáze může být prověřen pomocí hello portálu Azure.
4. Vytvoření clusteru HDInsight.
   
    tooexamine hello clusteru, můžete použít hello portál Azure nebo Azure PowerShell.
5. Předběžně zpracovat hello zdrojového datového souboru.
   
    V tomto kurzu můžete exportovat soubor protokolu log4j (soubor s oddělovači) a databázi Azure SQL tooan tabulku Hive. Hello souboru s oddělovači se nazývá */example/data/sample.log*. V kurzu hello jste viděli několik ukázky log4j protokolů. V souboru protokolu hello existují některé prázdné řádky a podobné toothese některé řádky:
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    To je v pořádku pro další příklady, které používají tato data, ale před jsme můžete importovat do hello Azure SQL database nebo SQL Server jsme musíte odebrat tyto výjimky. Sqoop export se nezdaří, pokud je prázdný řetězec nebo čáry s méně než hello počet polí definovaných v tabulce databáze Azure SQL hello elementy. Tabulka log4jlogs Hello má 7 řetězec typu pole.
   
    Tento postup vytvoří nový soubor v clusteru hello: tutorials/usesqoop/data/sample.log. tooexamine hello upravené datový soubor, můžete použít hello portálu Azure, nástroji Průzkumník Azure Storage nebo Azure PowerShell. [Začínáme s HDInsight] [ hdinsight-get-started] má kód ukázkové pro použití prostředí Azure PowerShell toodownload soubor a zobrazit obsah souboru hello.
6. Exportujte databáze Azure SQL data souboru toohello.
   
    zdrojový soubor Hello je tutorials/usesqoop/data/sample.log. Tabulka Hello kde hello dat je exportovaný toois nazývá log4jlogs.
   
   > [!NOTE]
   > Než informace o připojovacím řetězci by měly fungovat hello kroky v této části pro Azure SQL database nebo SQL Server. Tyto kroky testovali pomocí hello následující konfigurace:
   > 
   > * **Konfigurace point-to-site virtuální síť Azure**: virtuální síť připojená hello HDInsight clusteru tooa systému SQL Server v privátním datacentru. V tématu [konfigurace VPN typu Point-to-Site v hello portálu pro správu](../vpn-gateway/vpn-gateway-point-to-site-create.md) Další informace.
   > * **Azure HDInsight 3.1**: najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md) informace o vytváření clusteru s podporou ve virtuální síti.
   > * **SQL Server 2014**: nakonfigurovali tooallow ověřování a spuštěné hello VPN klienta konfigurace balíčku tooconnect bezpečně toohello virtuální sítě.
   > 
   > 
7. Exportujte databáze Azure SQL toohello tabulku Hive.
8. Importujte clusteru HDInsight toohello tabulky mobiledata hello.
   
    tooexamine hello upravené datový soubor, můžete použít hello portálu Azure, nástroji Průzkumník Azure Storage nebo Azure PowerShell.  [Začínáme s HDInsight] [ hdinsight-get-started] má kód ukázkové o použití prostředí Azure PowerShell toodownload soubor a zobrazit obsah souboru hello.

### <a name="hello-powershell-sample"></a>Ukázka Hello prostředí PowerShell
    # Prepare an Azure SQL database toobe used by hello Sqoop tutorial

    #region - provide hello following values

    $subscriptionID = "<Enter your Azure Subscription ID>"

    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"

    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"

    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"

    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"

    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region - Create tables
    Write-Host "Creating hello log4jlogs table and hello mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create hello mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()

    $conn.close()

    #endregion


    #region - Create HDInsight cluster

    Write-Host "Creating hello HDInsight cluster and hello dependent services ..." -ForegroundColor Green

    # Create hello default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create hello HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate hello cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - pre-process hello source file

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define hello connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing hello source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from hello source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into hello destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process hello source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove hello "java.lang.Exception" from hello first element of hello array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList tooremove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove hello lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write toohello destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from hello cluster toohello SQL database

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    $tableName_log4j = "log4jlogs"

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $exportDir_log4j = "/tutorials/usesqoop/data"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

    #endregion

    #region - export a Hive table

    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion

    #region - import a database

    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
