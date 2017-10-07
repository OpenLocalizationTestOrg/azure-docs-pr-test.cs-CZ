---
title: "aaaAnalyze letu zpoždění data s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak spouštět toouse jeden prostředí Windows PowerShell skriptu toocreate clusteru HDInsight, spouštět úlohy Hive, Sqoop úlohy a odstranění clusteru hello."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00e26aa9-82fb-4dbe-b87d-ffe8e39a5412
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6ebaee65d9b270e5dc2141dd1265011d372f497d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analýza dat zpoždění letu pomocí Hive v HDInsight
Hive zajišťuje spuštěných úloh Hadoop MapReduce prostřednictvím SQL jako skriptovacího jazyka nazvaného  *[HiveQL][hadoop-hiveql]*, který je možné použít ke shrnutí, dotazování, a analýze velkých objemů dat.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Windows. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Kroky, které pracují s clusterem se systémem Linux najdete v tématu [analyzovat data zpoždění letu pomocí Hive v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Jeden z největších výhod Azure HDInsight hello je hello oddělení dat úložiště a výpočty. HDInsight používá úložiště objektů Blob v Azure pro úložiště dat. Typické úlohy sestává ze tří částí:

1. **Ukládání dat v úložišti objektů Blob Azure.**  Například informace o počasí dat, data snímačů, webových protokolů a v takovém případě letu zpoždění data se uloží do úložiště objektů Blob v Azure.
2. **Spuštění úlohy.** Pokud je čas tooprocess hello dat, je spustit skript prostředí Windows PowerShell (nebo klientské aplikace) toocreate clusteru HDInsight, spouštění úloh a odstranění clusteru hello. Hello úlohy uložit výstupní data tooAzure úložiště objektů Blob. Hello výstupní data se uchovávají i po odstranění clusteru hello. Tímto způsobem platíte jenom to, na co mají použít.
3. **Načíst výstup hello z Azure Blob storage**, nebo v tomto kurzu exportovat hello data tooan Azure SQL database.

Hello následující diagram znázorňuje hello scénář a struktura hello tohoto kurzu:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

Všimněte si, zda hello čísla v diagramu hello odpovídají toohello názvů oddílů. **M** znamená hello hlavní proces. **A** znamená hello obsah v příloze hello.

hlavní část Hello hello kurzu se dozvíte, jak toouse jeden prostředí Windows PowerShell skriptu tooperform hello následující úkoly:

* Vytvoření clusteru HDInsight.
* Spusťte úlohu Hive v hello clusteru toocalculate průměrné zpoždění na letištích. Hello letu zpoždění data se ukládají v účtu úložiště objektů Blob v Azure.
* Spusťte Sqoop úlohy tooexport hello Hive úlohy výstup tooan Azure SQL database.
* Odstranění clusteru HDInsight hello.

V hello dodatky najdete hello pokyny pro nahrávání letu zpoždění dat, vytváření nebo odeslání řetězec dotazu Hive a příprava úlohy Sqoop hello hello Azure SQL database.

> [!NOTE]
> Hello kroky v tomto dokumentu jsou konkrétní na základě tooWindows clusterů HDInsight. Kroky, které pracují s clusterem se systémem Linux najdete v tématu [analyzovat data zpoždění letu používání Hive v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

### <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít hello následující položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Pracovní stanice s prostředím Azure PowerShell**.

    > [!IMPORTANT]
    > Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali. kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.
    >
    > Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell. Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.

**Soubory používané v tomto kurzu**

Tento kurz používá hello na časový výkon letecká společnost letu data z [výzkum a inovativní technologie správy, úřad Transport statistických nebo RITĚ][rita-website].
Kopie hello data byla úspěšně nahrál tooan kontejner úložiště objektů Blob v Azure s hello veřejného objektu Blob přístupová oprávnění.
Součástí vašeho skriptu prostředí PowerShell zkopíruje hello data z kontejneru hello veřejného objektu blob toohello výchozí kontejner na objektu blob ve vašem clusteru. Hello HiveQL skriptu je také zkopírován toohello stejný kontejner objektů Blob.
Pokud chcete, aby toolearn jak tooget nebo nahráváte hello data tooyour vlastní účet úložiště, a jak toocreate nebo nahráváte hello HiveQL skriptu souboru, najdete v části [příloha A](#appendix-a) a [příloha B](#appendix-b).

Hello následující tabulka uvádí soubory hello použité v tomto kurzu:

<table border="1">
<tr><th>Soubory</th><th>Popis</th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>soubor skriptu HiveQL Hello používá hello úlohy Hive. Tento skript je už nahraný tooan účtu úložiště Azure Blob s hello veřejný přístup. <a href="#appendix-b">Dodatek B</a> obsahuje pokyny k přípravě a odesílání tento soubor tooyour vlastní účtu Azure Blob storage.</td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Vstupní data pro úlohy Hive hello. Hello data nebyla nahrané tooan účtu úložiště Azure Blob s hello veřejný přístup. <a href="#appendix-a">Příloha A</a> obsahuje pokyny získávání hello dat a odesílání hello data tooyour vlastní účtu Azure Blob storage.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Hello výstupní cesta pro úlohy Hive hello. výchozí kontejner Hello se používá pro ukládání hello výstupní data.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Hello Hive úlohy stav složky na hello výchozí kontejner.</td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a>Vytvoření clusteru a spouštění úloh Hive nebo Sqoop
Hadoop MapReduce je dávkové zpracování. Hello většina nákladově efektivní způsob toorun úlohy Hive je toocreate clusteru s podporou pro úlohu hello a po dokončení úlohy hello odstranit úlohu hello. Hello následující skript obsahuje hello celého procesu.
Další informace o vytváření clusteru služby HDInsight a spuštění úloh Hive naleznete v tématu [vytvoření Hadoop clusterů v HDInsight] [ hdinsight-provision] a [používání Hive s HDInsight] [hdinsight-use-hive].

**toorun hello dotazů Hive pomocí prostředí Azure PowerShell**

1. Vytvoření tabulky Azure SQL database a hello pro výstup úlohy Sqoop hello pomocí pokynů hello v [příloha C](#appendix-c).
2. Otevřete Windows PowerShell ISE a spusťte následující skript hello:

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure hello follwing variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on hello version, hello folder can be different

    ###########################################
    # (Optional) configure hello following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter hello Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use hello cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

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
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare hello HiveQL script and source data
    ###########################################

    # Create hello temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download hello sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data toodefault container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit hello Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit hello Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete hello cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. Připojit databáze SQL tooyour a najdete v části zpoždění letů průměrná podle města v tabulce AvgDelays hello:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <a id="appendix-a"></a>Příloha A - nahrávání letu zpoždění data tooAzure úložiště objektů Blob
Odesílání hello datový soubor a soubory skript HiveQL hello (viz [příloha B](#appendix-b)) vyžaduje některé plánování. Rada Hello je toostore hello datové soubory a soubor HiveQL hello před vytváření clusteru služby HDInsight a spuštění úlohy Hive hello. Máte dvě možnosti:

* **Použití hello stejný účet úložiště Azure, který se použije jako výchozí systém souborů hello hello cluster HDInsight.** Protože clusteru HDInsight hello bude mít hello přístupový klíč účtu úložiště, nepotřebujete toomake žádné další změny.
* **Použijte jiný účet úložiště Azure z hello clusteru HDInsight, výchozí systém souborů.** Pokud se jedná o případ hello, musíte upravit hello vytvoření součástí hello skript zjištěno, že v prostředí Windows PowerShell [clusteru HDInsight se vytvoření a spuštění úlohy Hive nebo Sqoop](#runjob) toolink hello účtu úložiště jako další účet úložiště. Pokyny najdete v tématu [vytvoření Hadoop clusterů v HDInsight][hdinsight-provision]. Hello HDInsight cluster pak zná hello přístupový klíč pro hello účet úložiště.

> [!NOTE]
> Hello cestu k úložišti objektů Blob pro hello datový soubor je soubor skriptu HiveQL pevný programové v hello. Je třeba jej aktualizovat odpovídajícím způsobem.

**data pohybující se toodownload hello**

1. Procházet příliš[výzkum a inovativní technologie správy, úřad Transport statistických][rita-website].
2. Na stránce hello vyberte hello následující hodnoty:

    <table border="1">
    <tr><th>Name (Název)</th><th>Hodnota</th></tr>
    <tr><td>Filtr roku</td><td>2013 </td></tr>
    <tr><td>Filtrovat období</td><td>Leden</td></tr>
    <tr><td>Pole</td><td>*Rok*, *FlightDate*, *UniqueCarrier*, *poskytovatel*, *FlightNum*, *OriginAirportID* , *Původu*, *OriginCityName*, *OriginState*, *DestAirportID*, *cíle* , *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (zrušte zaškrtnutí všech ostatních polí)</td></tr>
    </table>
3.Klikněte na tlačítko **Stáhnout**.
4. Rozbalte soubor toohello hello **C:\Tutorials\FlightDelay\2013Data** složky. Každý soubor je soubor CSV a je přibližně 60GB.
5. Přejmenujte název souboru toohello hello měsíce, který obsahuje data pro hello. Například by s názvem hello soubor obsahující data leden hello *January.csv*.
6. Opakujte kroky 2 a 5 toodownload soubor pro každou hello dobu 12 měsíců v 2013. Budete potřebovat minimálně jeden kurzu hello toorun souboru.

**tooupload hello letu zpoždění data tooAzure úložiště objektů Blob**

1. Příprava hello parametry:

    <table border="1">
    <tr><th>Název proměnné</th><th>Poznámky</th></tr>
    <tr><td>$storageAccountName</td><td>účet úložiště Azure, kam chcete tooupload hello data Hello.</td></tr>
    <tr><td>$blobContainerName</td><td>kam chcete tooupload hello data kontejner objektů Blob Hello.</td></tr>
    </table>
2. Otevřete Azure PowerShell ISE.
3. Vložte následující skript do podokno skriptu hello hello:

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # hello source folder
    $destFolder = "tutorials/flightdelay/2013data"     #hello blob name prefix for hello files toobe uploaded
    #EndRegion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy hello file from local workstation tooAzure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName too$blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "hello source folder on hello workstation doesn't exist" -ForegroundColor Red
    }

    # List hello uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. Stiskněte klávesu **F5** toorun hello skriptu.

Pokud si zvolíte toouse jinou metodu pro nahrávání souborů hello, Zkontrolujte prosím, že cesta k souboru hello se kurzy/flightdelay nebo data. přístup k souborům hello Hello syntaxe je:

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Hello kurzy/flightdelay nebo data cesty je hello virtuální složku, kterou jste vytvořili, když jste odeslali soubory hello. Ověřte, jestli jsou 12 soubory, jeden pro každý měsíc.

> [!NOTE]
> Je třeba aktualizovat tooread dotaz Hive hello z nového umístění hello.
>
> Nakonfigurujete hello kontejneru přístup oprávnění toobe veřejné nebo vazby hello úložiště účet toohello clusteru HDInsight. Řetězec dotazu Hive hello, jinak nebude možné tooaccess hello datových souborů.

- - -

## <a id="appendix-b"></a>Dodatek B – vytvořit a odeslat skript HiveQL
Pomocí Azure PowerShell, můžete spustit více příkazy HiveQL jeden na čas, nebo balíček hello HiveQL příkaz do souboru skriptu. Tato část uvádí, jak toocreate skript HiveQL a nahrávání hello skriptu tooAzure úložiště objektů Blob pomocí Azure PowerShell. Hive vyžaduje hello HiveQL skripty toobe uložené v úložišti objektů Blob Azure.

Hello skript HiveQL provede hello následující:

1. **Odpojit tabulku delays_raw hello**, v případě, že hello tabulka již existuje.
2. **Vytvoří tabulku Hive externí delays_raw hello** odkazující toohello umístění úložiště objektů Blob s hello letu zpoždění soubory. Tento dotaz Určuje, že pole jsou oddělená "," a že řádky se ukončila příkazem "\n". To představuje problém, když hodnoty polí obsahovat čárky, protože podregistr nelze rozlišit mezi čárkami, který je oddělovačem polí a ten, který je součástí hodnotu pole (což je případ hello hodnoty v polích pro POČÁTEK\_MĚSTA\_název a cíl\_ MĚSTA\_název). tooaddress se hello dotazu vytvoří dočasné sloupce toohold data, která je nesprávně rozdělená do sloupců.
3. **Odpojit tabulku zpoždění hello**, v případě, že hello tabulka již existuje.
4. **Vytvoření tabulky zpoždění hello**. Je užitečné tooclean hello data před další zpracování. Tento dotaz vytvoří novou tabulku, *zpoždění*, z tabulky delays_raw hello. Všimněte si, že nejsou zkopírovány hello dočasné sloupce (jak je uvedeno nahoře) a že hello **substring** funkce je použité tooremove uvozovek z dat hello.
5. **Výpočetní hello průměrná počasí zpoždění a skupiny hello výsledky podle název města.** Je také výstup hello výsledky tooBlob úložiště. Poznámka: Tento dotaz hello dojde k odebrání apostrofy z hello dat a vyloučí řádky, kde hodnota hello **weather_delay** má hodnotu null. To je nezbytné, protože Sqoop, použít později v tomto kurzu, nemůže pracovat s těmito hodnotami řádně ve výchozím nastavení.

Úplný seznam příkazy HiveQL hello najdete v tématu [Hive Data Definition Language][hadoop-hiveql]. Každý příkaz HiveQL musí ukončit středníkem.

**soubor skriptu HiveQL toocreate**

1. Příprava hello parametry:

    <table border="1">
    <tr><th>Název proměnné</th><th>Poznámky</th></tr>
    <tr><td>$storageAccountName</td><td>Hello místo tooupload hello skript HiveQL k účtu Azure Storage.</td></tr>
    <tr><td>$blobContainerName</td><td>kontejner objektů Blob Hello místo tooupload hello skript HiveQL.</td></tr>
    </table>
2. Otevřete Azure PowerShell ISE.
3. Zkopírujte a vložte následující skript do podokno skriptu hello hello:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # hello HiveQL script file is exported as this file before it's uploaded tooBlob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # hello HiveQL script file will be uploaded tooBlob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by hello HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate hello file and file path

    # Check if a file with hello same file name already exists on hello workstation
    Write-Host "`nvalidating hello folder structure on hello workstation for saving hello HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'hello file, ' $hqlLocalFileName ', exists.  Do you want toooverwirte it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create hello folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write hello Hive script into a local file
    Write-Host "`nWriting hello Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload hello Hive script toohello default Blob container
    Write-Host "`nUploading hello Hive script toohello default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload hello file from local workstation tooBlob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

    Zde jsou hello proměnné používané ve skriptu hello:

   * **$hqlLocalFileName** -hello skriptu uloží soubor skriptu HiveQL hello místně před nahráním ho tooBlob úložiště. Toto je název souboru hello. Hello výchozí hodnota je <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
   * **$hqlBlobName** -Toto je hello HiveQL název souboru skriptu blob použít v hello úložiště objektů Blob v Azure. Hello výchozí hodnota je tutorials/flightdelay/flightdelays.hql. Protože hello soubor zapíše přímo tooAzure úložiště objektů Blob, není "/" na začátku hello hello název objektu blob. Pokud chcete soubor hello tooaccess z úložiště objektů Blob, budete potřebovat tooadd "/" na začátku hello hello název souboru.
   * **$srcDataFolder** a **$dstDataFolder** -= "kurzy/flightdelay nebo data" = "kurzy a flightdelay nebo výstupní"

- - -
## <a id="appendix-c"></a>Příloha C – Příprava Azure SQL database pro hello Sqoop výstup úlohy
**databáze SQL hello tooprepare (sloučení to s hello Sqoop skript)**

1. Příprava hello parametry:

    <table border="1">
    <tr><th>Název proměnné</th><th>Poznámky</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Název Hello hello server databáze Azure SQL. Zadejte nic toocreate nový server.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Hello přihlašovací jméno pro server databáze Azure SQL hello. Pokud $sqlDatabaseServerName stávajícího serveru, jsou hello přihlašovací jméno a heslo pro přihlášení použít tooauthenticate hello serveru. Jinak jsou použité toocreate nový server.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Hello přihlašovací heslo pro server databáze Azure SQL hello.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Tato hodnota se používá jenom v případě, že vytváříte nový server databáze Azure.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>databáze SQL Hello používá toocreate hello AvgDelays tabulky pro úlohu Sqoop hello. Ponechat prázdné vytvoří databázi s názvem HDISqoop. Název tabulky Hello hello Sqoop výstup úlohy je AvgDelays. </td></tr>
    </table>
2. Otevřete Azure PowerShell ISE.
3. Zkopírujte a vložte následující skript do podokno skriptu hello hello:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello region toocreate hello Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify hello database name if you have one created. Otherwise use "" toohave hello script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resouce group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command toocreate hello AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]
   > Hello skript používá služby representational stavu transfer (REST), http://bot.whatismyipaddress.com, tooretrieve externí IP adresu. Hello IP adresa se používá pro vytvoření pravidla brány firewall pro server databáze SQL.

    Zde jsou některé proměnné používané ve skriptu hello:

   * **$ipAddressRestService** -hello výchozí hodnota je http://bot.whatismyipaddress.com. Je veřejnou IP adresu služby REST pro získání externí IP adresu. Pokud chcete, můžete použít jiné služby. Hello externí IP adresu získat pomocí služby hello bude použité toocreate pravidlo brány firewall pro server databáze Azure SQL, aby mohli používat hello databáze z pracovní stanice (pomocí skriptu prostředí Windows PowerShell).
   * **$fireWallRuleName** -Toto je název hello hello pravidlo brány firewall pro server databáze Azure SQL hello. Hello výchozí název je <u>FlightDelay</u>. Pokud chcete, můžete ho změnit.
   * **$sqlDatabaseMaxSizeGB** – tato hodnota se používá jenom v případě, že vytváříte nový server databáze Azure SQL. Hello výchozí hodnota je 10GB. 10GB je dostačující pro účely tohoto kurzu.
   * **$sqlDatabaseName** – tato hodnota se používá jenom v případě, že vytváříte novou databázi Azure SQL. Hello výchozí hodnota je HDISqoop. Pokud přejmenujete, je nutné aktualizovat skriptu prostředí Windows PowerShell Sqoop hello odpovídajícím způsobem.
4. Stiskněte klávesu **F5** toorun hello skriptu.
5. Ověření výstupu skriptu hello. Ujistěte se, že skript hello proběhla úspěšně.

## <a id="nextsteps"></a> Další kroky
Nyní víte, jak tooupload soubor tooAzure úložiště objektů Blob, jak toopopulate podregistru tabulky pomocí hello dat z úložiště objektů Blob v Azure, jak toorun Hive dotazy a jak toouse Sqoop tooexport data z HDFS tooan Azure SQL database. toolearn více, najdete v části hello následující články:

* [Začínáme s HDInsight][hdinsight-get-started]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použijte Oozie s HDInsight][hdinsight-use-oozie]
* [Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
