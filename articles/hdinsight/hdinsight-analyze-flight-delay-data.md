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
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a><span data-ttu-id="499d3-103">Analýza dat zpoždění letu pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="499d3-103">Analyze flight delay data by using Hive in HDInsight</span></span>
<span data-ttu-id="499d3-104">Hive zajišťuje spuštěných úloh Hadoop MapReduce prostřednictvím SQL jako skriptovacího jazyka nazvaného  *[HiveQL][hadoop-hiveql]*, který je možné použít ke shrnutí, dotazování, a analýze velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="499d3-104">Hive provides a means of running Hadoop MapReduce jobs through an SQL-like scripting language called *[HiveQL][hadoop-hiveql]*, which can be applied towards summarizing, querying, and analyzing large volumes of data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="499d3-105">Hello kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="499d3-105">hello steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="499d3-106">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="499d3-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="499d3-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="499d3-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="499d3-108">Kroky, které pracují s clusterem se systémem Linux najdete v tématu [analyzovat data zpoždění letu pomocí Hive v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="499d3-108">For steps that work with a Linux-based cluster, see [Analyze flight delay data by using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).</span></span>

<span data-ttu-id="499d3-109">Jeden z největších výhod Azure HDInsight hello je hello oddělení dat úložiště a výpočty.</span><span class="sxs-lookup"><span data-stu-id="499d3-109">One of hello major benefits of Azure HDInsight is hello separation of data storage and compute.</span></span> <span data-ttu-id="499d3-110">HDInsight používá úložiště objektů Blob v Azure pro úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="499d3-110">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="499d3-111">Typické úlohy sestává ze tří částí:</span><span class="sxs-lookup"><span data-stu-id="499d3-111">A typical job involves three parts:</span></span>

1. <span data-ttu-id="499d3-112">**Ukládání dat v úložišti objektů Blob Azure.**</span><span class="sxs-lookup"><span data-stu-id="499d3-112">**Store data in Azure Blob storage.**</span></span>  <span data-ttu-id="499d3-113">Například informace o počasí dat, data snímačů, webových protokolů a v takovém případě letu zpoždění data se uloží do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="499d3-113">For example, weather data, sensor data, web logs, and in this case, flight delay data are saved into Azure Blob storage.</span></span>
2. <span data-ttu-id="499d3-114">**Spuštění úlohy.**</span><span class="sxs-lookup"><span data-stu-id="499d3-114">**Run jobs.**</span></span> <span data-ttu-id="499d3-115">Pokud je čas tooprocess hello dat, je spustit skript prostředí Windows PowerShell (nebo klientské aplikace) toocreate clusteru HDInsight, spouštění úloh a odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-115">When it is time tooprocess hello data, you run a Windows PowerShell script (or a client application) toocreate an HDInsight cluster, run jobs, and delete hello cluster.</span></span> <span data-ttu-id="499d3-116">Hello úlohy uložit výstupní data tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="499d3-116">hello jobs save output data tooAzure Blob storage.</span></span> <span data-ttu-id="499d3-117">Hello výstupní data se uchovávají i po odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-117">hello output data is retained even after hello cluster is deleted.</span></span> <span data-ttu-id="499d3-118">Tímto způsobem platíte jenom to, na co mají použít.</span><span class="sxs-lookup"><span data-stu-id="499d3-118">This way, you pay for only what you have consumed.</span></span>
3. <span data-ttu-id="499d3-119">**Načíst výstup hello z Azure Blob storage**, nebo v tomto kurzu exportovat hello data tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="499d3-119">**Retrieve hello output from Azure Blob storage**, or in this tutorial, export hello data tooan Azure SQL database.</span></span>

<span data-ttu-id="499d3-120">Hello následující diagram znázorňuje hello scénář a struktura hello tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="499d3-120">hello following diagram illustrates hello scenario and hello structure of this tutorial:</span></span>

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

<span data-ttu-id="499d3-122">Všimněte si, zda hello čísla v diagramu hello odpovídají toohello názvů oddílů.</span><span class="sxs-lookup"><span data-stu-id="499d3-122">Note that hello numbers in hello diagram correspond toohello section titles.</span></span> <span data-ttu-id="499d3-123">**M** znamená hello hlavní proces.</span><span class="sxs-lookup"><span data-stu-id="499d3-123">**M** stands for hello main process.</span></span> <span data-ttu-id="499d3-124">**A** znamená hello obsah v příloze hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-124">**A** stands for hello content in hello appendix.</span></span>

<span data-ttu-id="499d3-125">hlavní část Hello hello kurzu se dozvíte, jak toouse jeden prostředí Windows PowerShell skriptu tooperform hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="499d3-125">hello main portion of hello tutorial shows you how toouse one Windows PowerShell script tooperform hello following tasks:</span></span>

* <span data-ttu-id="499d3-126">Vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="499d3-126">Create an HDInsight cluster.</span></span>
* <span data-ttu-id="499d3-127">Spusťte úlohu Hive v hello clusteru toocalculate průměrné zpoždění na letištích.</span><span class="sxs-lookup"><span data-stu-id="499d3-127">Run a Hive job on hello cluster toocalculate average delays at airports.</span></span> <span data-ttu-id="499d3-128">Hello letu zpoždění data se ukládají v účtu úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="499d3-128">hello flight delay data is stored in an Azure Blob storage account.</span></span>
* <span data-ttu-id="499d3-129">Spusťte Sqoop úlohy tooexport hello Hive úlohy výstup tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="499d3-129">Run a Sqoop job tooexport hello Hive job output tooan Azure SQL database.</span></span>
* <span data-ttu-id="499d3-130">Odstranění clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-130">Delete hello HDInsight cluster.</span></span>

<span data-ttu-id="499d3-131">V hello dodatky najdete hello pokyny pro nahrávání letu zpoždění dat, vytváření nebo odeslání řetězec dotazu Hive a příprava úlohy Sqoop hello hello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="499d3-131">In hello appendixes, you can find hello instructions for uploading flight delay data, creating/uploading a Hive query string, and preparing hello Azure SQL database for hello Sqoop job.</span></span>

> [!NOTE]
> <span data-ttu-id="499d3-132">Hello kroky v tomto dokumentu jsou konkrétní na základě tooWindows clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="499d3-132">hello steps in this document are specific tooWindows-based HDInsight clusters.</span></span> <span data-ttu-id="499d3-133">Kroky, které pracují s clusterem se systémem Linux najdete v tématu [analyzovat data zpoždění letu používání Hive v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span><span class="sxs-lookup"><span data-stu-id="499d3-133">For steps that work with a Linux-based cluster, see [Analyze flight delay data using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span></span>

### <a name="prerequisites"></a><span data-ttu-id="499d3-134">Požadavky</span><span class="sxs-lookup"><span data-stu-id="499d3-134">Prerequisites</span></span>
<span data-ttu-id="499d3-135">Než začnete tento kurz, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="499d3-135">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="499d3-136">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="499d3-136">**An Azure subscription**.</span></span> <span data-ttu-id="499d3-137">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="499d3-137">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="499d3-138">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="499d3-138">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="499d3-139">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="499d3-139">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="499d3-140">kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="499d3-140">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="499d3-141">Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="499d3-141">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="499d3-142">Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="499d3-142">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

<span data-ttu-id="499d3-143">**Soubory používané v tomto kurzu**</span><span class="sxs-lookup"><span data-stu-id="499d3-143">**Files used in this tutorial**</span></span>

<span data-ttu-id="499d3-144">Tento kurz používá hello na časový výkon letecká společnost letu data z [výzkum a inovativní technologie správy, úřad Transport statistických nebo RITĚ][rita-website].</span><span class="sxs-lookup"><span data-stu-id="499d3-144">This tutorial uses hello on-time performance of airline flight data from [Research and Innovative Technology Administration, Bureau of Transportation Statistics or RITA][rita-website].</span></span>
<span data-ttu-id="499d3-145">Kopie hello data byla úspěšně nahrál tooan kontejner úložiště objektů Blob v Azure s hello veřejného objektu Blob přístupová oprávnění.</span><span class="sxs-lookup"><span data-stu-id="499d3-145">A copy of hello data has been uploaded tooan Azure Blob storage container with hello Public Blob access permission.</span></span>
<span data-ttu-id="499d3-146">Součástí vašeho skriptu prostředí PowerShell zkopíruje hello data z kontejneru hello veřejného objektu blob toohello výchozí kontejner na objektu blob ve vašem clusteru.</span><span class="sxs-lookup"><span data-stu-id="499d3-146">A part of your PowerShell script copies hello data from hello public blob container toohello default blob container of your cluster.</span></span> <span data-ttu-id="499d3-147">Hello HiveQL skriptu je také zkopírován toohello stejný kontejner objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="499d3-147">hello HiveQL script is also copied toohello same Blob container.</span></span>
<span data-ttu-id="499d3-148">Pokud chcete, aby toolearn jak tooget nebo nahráváte hello data tooyour vlastní účet úložiště, a jak toocreate nebo nahráváte hello HiveQL skriptu souboru, najdete v části [příloha A](#appendix-a) a [příloha B](#appendix-b).</span><span class="sxs-lookup"><span data-stu-id="499d3-148">If you want toolearn how tooget/upload hello data tooyour own Storage account, and how toocreate/upload hello HiveQL script file, see [Appendix A](#appendix-a) and [Appendix B](#appendix-b).</span></span>

<span data-ttu-id="499d3-149">Hello následující tabulka uvádí soubory hello použité v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="499d3-149">hello following table lists hello files used in this tutorial:</span></span>

<table border="1">
<tr><th><span data-ttu-id="499d3-150">Soubory</span><span class="sxs-lookup"><span data-stu-id="499d3-150">Files</span></span></th><th><span data-ttu-id="499d3-151">Popis</span><span class="sxs-lookup"><span data-stu-id="499d3-151">Description</span></span></th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td><span data-ttu-id="499d3-152">soubor skriptu HiveQL Hello používá hello úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="499d3-152">hello HiveQL script file used by hello Hive job.</span></span> <span data-ttu-id="499d3-153">Tento skript je už nahraný tooan účtu úložiště Azure Blob s hello veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="499d3-153">This script has been uploaded tooan Azure Blob storage account with hello public access.</span></span> <span data-ttu-id="499d3-154"><a href="#appendix-b">Dodatek B</a> obsahuje pokyny k přípravě a odesílání tento soubor tooyour vlastní účtu Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="499d3-154"><a href="#appendix-b">Appendix B</a> has instructions on preparing and uploading this file tooyour own Azure Blob storage account.</span></span></td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td><span data-ttu-id="499d3-155">Vstupní data pro úlohy Hive hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-155">Input data for hello Hive job.</span></span> <span data-ttu-id="499d3-156">Hello data nebyla nahrané tooan účtu úložiště Azure Blob s hello veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="499d3-156">hello data has been uploaded tooan Azure Blob storage account with hello public access.</span></span> <span data-ttu-id="499d3-157"><a href="#appendix-a">Příloha A</a> obsahuje pokyny získávání hello dat a odesílání hello data tooyour vlastní účtu Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="499d3-157"><a href="#appendix-a">Appendix A</a> has instructions on getting hello data and uploading hello data tooyour own Azure Blob storage account.</span></span></td></tr>
<tr><td><span data-ttu-id="499d3-158">\tutorials\flightdelays\output</span><span class="sxs-lookup"><span data-stu-id="499d3-158">\tutorials\flightdelays\output</span></span></td><td><span data-ttu-id="499d3-159">Hello výstupní cesta pro úlohy Hive hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-159">hello output path for hello Hive job.</span></span> <span data-ttu-id="499d3-160">výchozí kontejner Hello se používá pro ukládání hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="499d3-160">hello default container is used for storing hello output data.</span></span></td></tr>
<tr><td><span data-ttu-id="499d3-161">\tutorials\flightdelays\jobstatus</span><span class="sxs-lookup"><span data-stu-id="499d3-161">\tutorials\flightdelays\jobstatus</span></span></td><td><span data-ttu-id="499d3-162">Hello Hive úlohy stav složky na hello výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="499d3-162">hello Hive job status folder on hello default container.</span></span></td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a><span data-ttu-id="499d3-163">Vytvoření clusteru a spouštění úloh Hive nebo Sqoop</span><span class="sxs-lookup"><span data-stu-id="499d3-163">Create cluster and run Hive/Sqoop jobs</span></span>
<span data-ttu-id="499d3-164">Hadoop MapReduce je dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="499d3-164">Hadoop MapReduce is batch processing.</span></span> <span data-ttu-id="499d3-165">Hello většina nákladově efektivní způsob toorun úlohy Hive je toocreate clusteru s podporou pro úlohu hello a po dokončení úlohy hello odstranit úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-165">hello most cost-effective way toorun a Hive job is toocreate a cluster for hello job, and delete hello job after hello job is completed.</span></span> <span data-ttu-id="499d3-166">Hello následující skript obsahuje hello celého procesu.</span><span class="sxs-lookup"><span data-stu-id="499d3-166">hello following script covers hello whole process.</span></span>
<span data-ttu-id="499d3-167">Další informace o vytváření clusteru služby HDInsight a spuštění úloh Hive naleznete v tématu [vytvoření Hadoop clusterů v HDInsight] [ hdinsight-provision] a [používání Hive s HDInsight] [hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="499d3-167">For more information on creating an HDInsight cluster and running Hive jobs, see [Create Hadoop clusters in HDInsight][hdinsight-provision] and [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

<span data-ttu-id="499d3-168">**toorun hello dotazů Hive pomocí prostředí Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="499d3-168">**toorun hello Hive queries by Azure PowerShell**</span></span>

1. <span data-ttu-id="499d3-169">Vytvoření tabulky Azure SQL database a hello pro výstup úlohy Sqoop hello pomocí pokynů hello v [příloha C](#appendix-c).</span><span class="sxs-lookup"><span data-stu-id="499d3-169">Create an Azure SQL database and hello table for hello Sqoop job output by using hello instructions in [Appendix C](#appendix-c).</span></span>
2. <span data-ttu-id="499d3-170">Otevřete Windows PowerShell ISE a spusťte následující skript hello:</span><span class="sxs-lookup"><span data-stu-id="499d3-170">Open Windows PowerShell ISE, and run hello following script:</span></span>

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
3. <span data-ttu-id="499d3-171">Připojit databáze SQL tooyour a najdete v části zpoždění letů průměrná podle města v tabulce AvgDelays hello:</span><span class="sxs-lookup"><span data-stu-id="499d3-171">Connect tooyour SQL database and see average flight delays by city in hello AvgDelays table:</span></span>

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <span data-ttu-id="499d3-173"><a id="appendix-a"></a>Příloha A - nahrávání letu zpoždění data tooAzure úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="499d3-173"><a id="appendix-a"></a>Appendix A - Upload flight delay data tooAzure Blob storage</span></span>
<span data-ttu-id="499d3-174">Odesílání hello datový soubor a soubory skript HiveQL hello (viz [příloha B](#appendix-b)) vyžaduje některé plánování.</span><span class="sxs-lookup"><span data-stu-id="499d3-174">Uploading hello data file and hello HiveQL script files (see [Appendix B](#appendix-b)) requires some planning.</span></span> <span data-ttu-id="499d3-175">Rada Hello je toostore hello datové soubory a soubor HiveQL hello před vytváření clusteru služby HDInsight a spuštění úlohy Hive hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-175">hello idea is toostore hello data files and hello HiveQL file before creating an HDInsight cluster and running hello Hive job.</span></span> <span data-ttu-id="499d3-176">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="499d3-176">You have two options:</span></span>

* <span data-ttu-id="499d3-177">**Použití hello stejný účet úložiště Azure, který se použije jako výchozí systém souborů hello hello cluster HDInsight.**</span><span class="sxs-lookup"><span data-stu-id="499d3-177">**Use hello same Azure Storage account that will be used by hello HDInsight cluster as hello default file system.**</span></span> <span data-ttu-id="499d3-178">Protože clusteru HDInsight hello bude mít hello přístupový klíč účtu úložiště, nepotřebujete toomake žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="499d3-178">Because hello HDInsight cluster will have hello Storage account access key, you don't need toomake any additional changes.</span></span>
* <span data-ttu-id="499d3-179">**Použijte jiný účet úložiště Azure z hello clusteru HDInsight, výchozí systém souborů.**</span><span class="sxs-lookup"><span data-stu-id="499d3-179">**Use a different Azure Storage account from hello HDInsight cluster default file system.**</span></span> <span data-ttu-id="499d3-180">Pokud se jedná o případ hello, musíte upravit hello vytvoření součástí hello skript zjištěno, že v prostředí Windows PowerShell [clusteru HDInsight se vytvoření a spuštění úlohy Hive nebo Sqoop](#runjob) toolink hello účtu úložiště jako další účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="499d3-180">If this is hello case, you must modify hello creation part of hello Windows PowerShell script found in [Create HDInsight cluster and run Hive/Sqoop jobs](#runjob) toolink hello Storage account as an additional Storage account.</span></span> <span data-ttu-id="499d3-181">Pokyny najdete v tématu [vytvoření Hadoop clusterů v HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="499d3-181">For instructions, see [Create Hadoop clusters in HDInsight][hdinsight-provision].</span></span> <span data-ttu-id="499d3-182">Hello HDInsight cluster pak zná hello přístupový klíč pro hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="499d3-182">hello HDInsight cluster then knows hello access key for hello Storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="499d3-183">Hello cestu k úložišti objektů Blob pro hello datový soubor je soubor skriptu HiveQL pevný programové v hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-183">hello Blob storage path for hello data file is hard coded in hello HiveQL script file.</span></span> <span data-ttu-id="499d3-184">Je třeba jej aktualizovat odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="499d3-184">You must update it accordingly.</span></span>

<span data-ttu-id="499d3-185">**data pohybující se toodownload hello**</span><span class="sxs-lookup"><span data-stu-id="499d3-185">**toodownload hello flight data**</span></span>

1. <span data-ttu-id="499d3-186">Procházet příliš[výzkum a inovativní technologie správy, úřad Transport statistických][rita-website].</span><span class="sxs-lookup"><span data-stu-id="499d3-186">Browse too[Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>
2. <span data-ttu-id="499d3-187">Na stránce hello vyberte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="499d3-187">On hello page, select hello following values:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="499d3-188">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="499d3-188">Name</span></span></th><th><span data-ttu-id="499d3-189">Hodnota</span><span class="sxs-lookup"><span data-stu-id="499d3-189">Value</span></span></th></tr>
    <tr><td><span data-ttu-id="499d3-190">Filtr roku</span><span class="sxs-lookup"><span data-stu-id="499d3-190">Filter Year</span></span></td><td><span data-ttu-id="499d3-191">2013</span><span class="sxs-lookup"><span data-stu-id="499d3-191">2013</span></span> </td></tr>
    <tr><td><span data-ttu-id="499d3-192">Filtrovat období</span><span class="sxs-lookup"><span data-stu-id="499d3-192">Filter Period</span></span></td><td><span data-ttu-id="499d3-193">Leden</span><span class="sxs-lookup"><span data-stu-id="499d3-193">January</span></span></td></tr>
    <tr><td><span data-ttu-id="499d3-194">Pole</span><span class="sxs-lookup"><span data-stu-id="499d3-194">Fields</span></span></td><td><span data-ttu-id="499d3-195">*Rok*, *FlightDate*, *UniqueCarrier*, *poskytovatel*, *FlightNum*, *OriginAirportID* , *Původu*, *OriginCityName*, *OriginState*, *DestAirportID*, *cíle* , *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (zrušte zaškrtnutí všech ostatních polí)</span><span class="sxs-lookup"><span data-stu-id="499d3-195">*Year*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (clear all other fields)</span></span></td></tr>
    </table><span data-ttu-id="499d3-196">
3.Klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="499d3-196">
3. Click **Download**.</span></span>
<span data-ttu-id="499d3-197">4.</span><span class="sxs-lookup"><span data-stu-id="499d3-197">4.</span></span> <span data-ttu-id="499d3-198">Rozbalte soubor toohello hello **C:\Tutorials\FlightDelay\2013Data** složky.</span><span class="sxs-lookup"><span data-stu-id="499d3-198">Unzip hello file toohello **C:\Tutorials\FlightDelay\2013Data** folder.</span></span> <span data-ttu-id="499d3-199">Každý soubor je soubor CSV a je přibližně 60GB.</span><span class="sxs-lookup"><span data-stu-id="499d3-199">Each file is a CSV file and is approximately 60GB in size.</span></span>
<span data-ttu-id="499d3-200">5.</span><span class="sxs-lookup"><span data-stu-id="499d3-200">5.</span></span> <span data-ttu-id="499d3-201">Přejmenujte název souboru toohello hello měsíce, který obsahuje data pro hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-201">Rename hello file toohello name of hello month that it contains data for.</span></span> <span data-ttu-id="499d3-202">Například by s názvem hello soubor obsahující data leden hello *January.csv*.</span><span class="sxs-lookup"><span data-stu-id="499d3-202">For example, hello file containing hello January data would be named *January.csv*.</span></span>
<span data-ttu-id="499d3-203">6.</span><span class="sxs-lookup"><span data-stu-id="499d3-203">6.</span></span> <span data-ttu-id="499d3-204">Opakujte kroky 2 a 5 toodownload soubor pro každou hello dobu 12 měsíců v 2013.</span><span class="sxs-lookup"><span data-stu-id="499d3-204">Repeat steps 2 and 5 toodownload a file for each of hello 12 months in 2013.</span></span> <span data-ttu-id="499d3-205">Budete potřebovat minimálně jeden kurzu hello toorun souboru.</span><span class="sxs-lookup"><span data-stu-id="499d3-205">You will need a minimum of one file toorun hello tutorial.</span></span>

<span data-ttu-id="499d3-206">**tooupload hello letu zpoždění data tooAzure úložiště objektů Blob**</span><span class="sxs-lookup"><span data-stu-id="499d3-206">**tooupload hello flight delay data tooAzure Blob storage**</span></span>

1. <span data-ttu-id="499d3-207">Příprava hello parametry:</span><span class="sxs-lookup"><span data-stu-id="499d3-207">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="499d3-208">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="499d3-208">Variable Name</span></span></th><th><span data-ttu-id="499d3-209">Poznámky</span><span class="sxs-lookup"><span data-stu-id="499d3-209">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="499d3-210">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="499d3-210">$storageAccountName</span></span></td><td><span data-ttu-id="499d3-211">účet úložiště Azure, kam chcete tooupload hello data Hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-211">hello Azure Storage account where you want tooupload hello data to.</span></span></td></tr>
    <tr><td><span data-ttu-id="499d3-212">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="499d3-212">$blobContainerName</span></span></td><td><span data-ttu-id="499d3-213">kam chcete tooupload hello data kontejner objektů Blob Hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-213">hello Blob container where you want tooupload hello data to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="499d3-214">Otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="499d3-214">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="499d3-215">3.</span><span class="sxs-lookup"><span data-stu-id="499d3-215">3.</span></span> <span data-ttu-id="499d3-216">Vložte následující skript do podokno skriptu hello hello:</span><span class="sxs-lookup"><span data-stu-id="499d3-216">Paste hello following script into hello script pane:</span></span>

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
4. <span data-ttu-id="499d3-217">Stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="499d3-217">Press **F5** toorun hello script.</span></span>

<span data-ttu-id="499d3-218">Pokud si zvolíte toouse jinou metodu pro nahrávání souborů hello, Zkontrolujte prosím, že cesta k souboru hello se kurzy/flightdelay nebo data.</span><span class="sxs-lookup"><span data-stu-id="499d3-218">If you choose toouse a different method for uploading hello files, please make sure hello file path is tutorials/flightdelay/data.</span></span> <span data-ttu-id="499d3-219">přístup k souborům hello Hello syntaxe je:</span><span class="sxs-lookup"><span data-stu-id="499d3-219">hello syntax for accessing hello files is:</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

<span data-ttu-id="499d3-220">Hello kurzy/flightdelay nebo data cesty je hello virtuální složku, kterou jste vytvořili, když jste odeslali soubory hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-220">hello path tutorials/flightdelay/data is hello virtual folder you created when you uploaded hello files.</span></span> <span data-ttu-id="499d3-221">Ověřte, jestli jsou 12 soubory, jeden pro každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="499d3-221">Verify that there are 12 files, one for each month.</span></span>

> [!NOTE]
> <span data-ttu-id="499d3-222">Je třeba aktualizovat tooread dotaz Hive hello z nového umístění hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-222">You must update hello Hive query tooread from hello new location.</span></span>
>
> <span data-ttu-id="499d3-223">Nakonfigurujete hello kontejneru přístup oprávnění toobe veřejné nebo vazby hello úložiště účet toohello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="499d3-223">You must either configure hello container access permission toobe public or bind hello Storage account toohello HDInsight cluster.</span></span> <span data-ttu-id="499d3-224">Řetězec dotazu Hive hello, jinak nebude možné tooaccess hello datových souborů.</span><span class="sxs-lookup"><span data-stu-id="499d3-224">Otherwise, hello Hive query string will not be able tooaccess hello data files.</span></span>

- - -

## <span data-ttu-id="499d3-225"><a id="appendix-b"></a>Dodatek B – vytvořit a odeslat skript HiveQL</span><span class="sxs-lookup"><span data-stu-id="499d3-225"><a id="appendix-b"></a>Appendix B - Create and upload a HiveQL script</span></span>
<span data-ttu-id="499d3-226">Pomocí Azure PowerShell, můžete spustit více příkazy HiveQL jeden na čas, nebo balíček hello HiveQL příkaz do souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="499d3-226">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package hello HiveQL statement into a script file.</span></span> <span data-ttu-id="499d3-227">Tato část uvádí, jak toocreate skript HiveQL a nahrávání hello skriptu tooAzure úložiště objektů Blob pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="499d3-227">This section shows you how toocreate a HiveQL script and upload hello script tooAzure Blob storage by using Azure PowerShell.</span></span> <span data-ttu-id="499d3-228">Hive vyžaduje hello HiveQL skripty toobe uložené v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="499d3-228">Hive requires hello HiveQL scripts toobe stored in Azure Blob storage.</span></span>

<span data-ttu-id="499d3-229">Hello skript HiveQL provede hello následující:</span><span class="sxs-lookup"><span data-stu-id="499d3-229">hello HiveQL script will perform hello following:</span></span>

1. <span data-ttu-id="499d3-230">**Odpojit tabulku delays_raw hello**, v případě, že hello tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="499d3-230">**Drop hello delays_raw table**, in case hello table already exists.</span></span>
2. <span data-ttu-id="499d3-231">**Vytvoří tabulku Hive externí delays_raw hello** odkazující toohello umístění úložiště objektů Blob s hello letu zpoždění soubory.</span><span class="sxs-lookup"><span data-stu-id="499d3-231">**Create hello delays_raw external Hive table** pointing toohello Blob storage location with hello flight delay files.</span></span> <span data-ttu-id="499d3-232">Tento dotaz Určuje, že pole jsou oddělená "," a že řádky se ukončila příkazem "\n".</span><span class="sxs-lookup"><span data-stu-id="499d3-232">This query specifies that fields are delimited by "," and that lines are terminated by "\n".</span></span> <span data-ttu-id="499d3-233">To představuje problém, když hodnoty polí obsahovat čárky, protože podregistr nelze rozlišit mezi čárkami, který je oddělovačem polí a ten, který je součástí hodnotu pole (což je případ hello hodnoty v polích pro POČÁTEK\_MĚSTA\_název a cíl\_ MĚSTA\_název).</span><span class="sxs-lookup"><span data-stu-id="499d3-233">This poses a problem when field values contain commas because Hive cannot differentiate between a comma that is a field delimiter and a one that is part of a field value (which is hello case in field values for ORIGIN\_CITY\_NAME and DEST\_CITY\_NAME).</span></span> <span data-ttu-id="499d3-234">tooaddress se hello dotazu vytvoří dočasné sloupce toohold data, která je nesprávně rozdělená do sloupců.</span><span class="sxs-lookup"><span data-stu-id="499d3-234">tooaddress this, hello query creates TEMP columns toohold data that is incorrectly split into columns.</span></span>
3. <span data-ttu-id="499d3-235">**Odpojit tabulku zpoždění hello**, v případě, že hello tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="499d3-235">**Drop hello delays table**, in case hello table already exists.</span></span>
4. <span data-ttu-id="499d3-236">**Vytvoření tabulky zpoždění hello**.</span><span class="sxs-lookup"><span data-stu-id="499d3-236">**Create hello delays table**.</span></span> <span data-ttu-id="499d3-237">Je užitečné tooclean hello data před další zpracování.</span><span class="sxs-lookup"><span data-stu-id="499d3-237">It is helpful tooclean up hello data before further processing.</span></span> <span data-ttu-id="499d3-238">Tento dotaz vytvoří novou tabulku, *zpoždění*, z tabulky delays_raw hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-238">This query creates a new table, *delays*, from hello delays_raw table.</span></span> <span data-ttu-id="499d3-239">Všimněte si, že nejsou zkopírovány hello dočasné sloupce (jak je uvedeno nahoře) a že hello **substring** funkce je použité tooremove uvozovek z dat hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-239">Note that hello TEMP columns (as mentioned previously) are not copied, and that hello **substring** function is used tooremove quotation marks from hello data.</span></span>
5. <span data-ttu-id="499d3-240">**Výpočetní hello průměrná počasí zpoždění a skupiny hello výsledky podle název města.**</span><span class="sxs-lookup"><span data-stu-id="499d3-240">**Compute hello average weather delay and groups hello results by city name.**</span></span> <span data-ttu-id="499d3-241">Je také výstup hello výsledky tooBlob úložiště.</span><span class="sxs-lookup"><span data-stu-id="499d3-241">It will also output hello results tooBlob storage.</span></span> <span data-ttu-id="499d3-242">Poznámka: Tento dotaz hello dojde k odebrání apostrofy z hello dat a vyloučí řádky, kde hodnota hello **weather_delay** má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="499d3-242">Note that hello query will remove apostrophes from hello data and will exclude rows where hello value for **weather_delay** is null.</span></span> <span data-ttu-id="499d3-243">To je nezbytné, protože Sqoop, použít později v tomto kurzu, nemůže pracovat s těmito hodnotami řádně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="499d3-243">This is necessary because Sqoop, used later in this tutorial, doesn't handle those values gracefully by default.</span></span>

<span data-ttu-id="499d3-244">Úplný seznam příkazy HiveQL hello najdete v tématu [Hive Data Definition Language][hadoop-hiveql].</span><span class="sxs-lookup"><span data-stu-id="499d3-244">For a full list of hello HiveQL commands, see [Hive Data Definition Language][hadoop-hiveql].</span></span> <span data-ttu-id="499d3-245">Každý příkaz HiveQL musí ukončit středníkem.</span><span class="sxs-lookup"><span data-stu-id="499d3-245">Each HiveQL command must terminate with a semicolon.</span></span>

<span data-ttu-id="499d3-246">**soubor skriptu HiveQL toocreate**</span><span class="sxs-lookup"><span data-stu-id="499d3-246">**toocreate a HiveQL script file**</span></span>

1. <span data-ttu-id="499d3-247">Příprava hello parametry:</span><span class="sxs-lookup"><span data-stu-id="499d3-247">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="499d3-248">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="499d3-248">Variable Name</span></span></th><th><span data-ttu-id="499d3-249">Poznámky</span><span class="sxs-lookup"><span data-stu-id="499d3-249">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="499d3-250">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="499d3-250">$storageAccountName</span></span></td><td><span data-ttu-id="499d3-251">Hello místo tooupload hello skript HiveQL k účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="499d3-251">hello Azure Storage account where you want tooupload hello HiveQL script to.</span></span></td></tr>
    <tr><td><span data-ttu-id="499d3-252">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="499d3-252">$blobContainerName</span></span></td><td><span data-ttu-id="499d3-253">kontejner objektů Blob Hello místo tooupload hello skript HiveQL.</span><span class="sxs-lookup"><span data-stu-id="499d3-253">hello Blob container where you want tooupload hello HiveQL script to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="499d3-254">Otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="499d3-254">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="499d3-255">3.</span><span class="sxs-lookup"><span data-stu-id="499d3-255">3.</span></span> <span data-ttu-id="499d3-256">Zkopírujte a vložte následující skript do podokno skriptu hello hello:</span><span class="sxs-lookup"><span data-stu-id="499d3-256">Copy and paste hello following script into hello script pane:</span></span>

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

    <span data-ttu-id="499d3-257">Zde jsou hello proměnné používané ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="499d3-257">Here are hello variables used in hello script:</span></span>

   * <span data-ttu-id="499d3-258">**$hqlLocalFileName** -hello skriptu uloží soubor skriptu HiveQL hello místně před nahráním ho tooBlob úložiště.</span><span class="sxs-lookup"><span data-stu-id="499d3-258">**$hqlLocalFileName** - hello script saves hello HiveQL script file locally before uploading it tooBlob storage.</span></span> <span data-ttu-id="499d3-259">Toto je název souboru hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-259">This is hello file name.</span></span> <span data-ttu-id="499d3-260">Hello výchozí hodnota je <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span><span class="sxs-lookup"><span data-stu-id="499d3-260">hello default value is <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span></span>
   * <span data-ttu-id="499d3-261">**$hqlBlobName** -Toto je hello HiveQL název souboru skriptu blob použít v hello úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="499d3-261">**$hqlBlobName** - This is hello HiveQL script file blob name used in hello Azure Blob storage.</span></span> <span data-ttu-id="499d3-262">Hello výchozí hodnota je tutorials/flightdelay/flightdelays.hql.</span><span class="sxs-lookup"><span data-stu-id="499d3-262">hello default value is tutorials/flightdelay/flightdelays.hql.</span></span> <span data-ttu-id="499d3-263">Protože hello soubor zapíše přímo tooAzure úložiště objektů Blob, není "/" na začátku hello hello název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="499d3-263">Because hello file will be written directly tooAzure Blob storage, there is NOT a "/" at hello beginning of hello blob name.</span></span> <span data-ttu-id="499d3-264">Pokud chcete soubor hello tooaccess z úložiště objektů Blob, budete potřebovat tooadd "/" na začátku hello hello název souboru.</span><span class="sxs-lookup"><span data-stu-id="499d3-264">If you want tooaccess hello file from Blob storage, you will need tooadd a "/" at hello beginning of hello file name.</span></span>
   * <span data-ttu-id="499d3-265">**$srcDataFolder** a **$dstDataFolder** -= "kurzy/flightdelay nebo data" = "kurzy a flightdelay nebo výstupní"</span><span class="sxs-lookup"><span data-stu-id="499d3-265">**$srcDataFolder** and **$dstDataFolder** - = "tutorials/flightdelay/data" = "tutorials/flightdelay/output"</span></span>

- - -
## <span data-ttu-id="499d3-266"><a id="appendix-c"></a>Příloha C – Příprava Azure SQL database pro hello Sqoop výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="499d3-266"><a id="appendix-c"></a>Appendix C - Prepare an Azure SQL database for hello Sqoop job output</span></span>
<span data-ttu-id="499d3-267">**databáze SQL hello tooprepare (sloučení to s hello Sqoop skript)**</span><span class="sxs-lookup"><span data-stu-id="499d3-267">**tooprepare hello SQL database (merge this with hello Sqoop script)**</span></span>

1. <span data-ttu-id="499d3-268">Příprava hello parametry:</span><span class="sxs-lookup"><span data-stu-id="499d3-268">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="499d3-269">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="499d3-269">Variable Name</span></span></th><th><span data-ttu-id="499d3-270">Poznámky</span><span class="sxs-lookup"><span data-stu-id="499d3-270">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="499d3-271">$sqlDatabaseServerName</span><span class="sxs-lookup"><span data-stu-id="499d3-271">$sqlDatabaseServerName</span></span></td><td><span data-ttu-id="499d3-272">Název Hello hello server databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="499d3-272">hello name of hello Azure SQL database server.</span></span> <span data-ttu-id="499d3-273">Zadejte nic toocreate nový server.</span><span class="sxs-lookup"><span data-stu-id="499d3-273">Enter nothing toocreate a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="499d3-274">$sqlDatabaseUsername</span><span class="sxs-lookup"><span data-stu-id="499d3-274">$sqlDatabaseUsername</span></span></td><td><span data-ttu-id="499d3-275">Hello přihlašovací jméno pro server databáze Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-275">hello login name for hello Azure SQL database server.</span></span> <span data-ttu-id="499d3-276">Pokud $sqlDatabaseServerName stávajícího serveru, jsou hello přihlašovací jméno a heslo pro přihlášení použít tooauthenticate hello serveru.</span><span class="sxs-lookup"><span data-stu-id="499d3-276">If $sqlDatabaseServerName is an existing server, hello login and login password are used tooauthenticate with hello server.</span></span> <span data-ttu-id="499d3-277">Jinak jsou použité toocreate nový server.</span><span class="sxs-lookup"><span data-stu-id="499d3-277">Otherwise they are used toocreate a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="499d3-278">$sqlDatabasePassword</span><span class="sxs-lookup"><span data-stu-id="499d3-278">$sqlDatabasePassword</span></span></td><td><span data-ttu-id="499d3-279">Hello přihlašovací heslo pro server databáze Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-279">hello login password for hello Azure SQL database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="499d3-280">$sqlDatabaseLocation</span><span class="sxs-lookup"><span data-stu-id="499d3-280">$sqlDatabaseLocation</span></span></td><td><span data-ttu-id="499d3-281">Tato hodnota se používá jenom v případě, že vytváříte nový server databáze Azure.</span><span class="sxs-lookup"><span data-stu-id="499d3-281">This value is used only when you're creating a new Azure database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="499d3-282">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="499d3-282">$sqlDatabaseName</span></span></td><td><span data-ttu-id="499d3-283">databáze SQL Hello používá toocreate hello AvgDelays tabulky pro úlohu Sqoop hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-283">hello SQL database used toocreate hello AvgDelays table for hello Sqoop job.</span></span> <span data-ttu-id="499d3-284">Ponechat prázdné vytvoří databázi s názvem HDISqoop.</span><span class="sxs-lookup"><span data-stu-id="499d3-284">Leaving it blank will create a database called HDISqoop.</span></span> <span data-ttu-id="499d3-285">Název tabulky Hello hello Sqoop výstup úlohy je AvgDelays.</span><span class="sxs-lookup"><span data-stu-id="499d3-285">hello table name for hello Sqoop job output is AvgDelays.</span></span> </td></tr>
    </table>
2. <span data-ttu-id="499d3-286">Otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="499d3-286">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="499d3-287">3.</span><span class="sxs-lookup"><span data-stu-id="499d3-287">3.</span></span> <span data-ttu-id="499d3-288">Zkopírujte a vložte následující skript do podokno skriptu hello hello:</span><span class="sxs-lookup"><span data-stu-id="499d3-288">Copy and paste hello following script into hello script pane:</span></span>

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
   > <span data-ttu-id="499d3-289">Hello skript používá služby representational stavu transfer (REST), http://bot.whatismyipaddress.com, tooretrieve externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="499d3-289">hello script uses a representational state transfer (REST) service, http://bot.whatismyipaddress.com, tooretrieve your external IP address.</span></span> <span data-ttu-id="499d3-290">Hello IP adresa se používá pro vytvoření pravidla brány firewall pro server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="499d3-290">hello IP address is used for creating a firewall rule for your SQL database server.</span></span>

    <span data-ttu-id="499d3-291">Zde jsou některé proměnné používané ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="499d3-291">Here are some variables used in hello script:</span></span>

   * <span data-ttu-id="499d3-292">**$ipAddressRestService** -hello výchozí hodnota je http://bot.whatismyipaddress.com. Je veřejnou IP adresu služby REST pro získání externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="499d3-292">**$ipAddressRestService** - hello default value is http://bot.whatismyipaddress.com. It is a public IP address REST service for getting your external IP address.</span></span> <span data-ttu-id="499d3-293">Pokud chcete, můžete použít jiné služby.</span><span class="sxs-lookup"><span data-stu-id="499d3-293">You can use other services if you want.</span></span> <span data-ttu-id="499d3-294">Hello externí IP adresu získat pomocí služby hello bude použité toocreate pravidlo brány firewall pro server databáze Azure SQL, aby mohli používat hello databáze z pracovní stanice (pomocí skriptu prostředí Windows PowerShell).</span><span class="sxs-lookup"><span data-stu-id="499d3-294">hello external IP address retrieved through hello service will be used toocreate a firewall rule for your Azure SQL database server, so that you can access hello database from your workstation (by using a Windows PowerShell script).</span></span>
   * <span data-ttu-id="499d3-295">**$fireWallRuleName** -Toto je název hello hello pravidlo brány firewall pro server databáze Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-295">**$fireWallRuleName** - This is hello name of hello firewall rule for hello Azure SQL database server.</span></span> <span data-ttu-id="499d3-296">Hello výchozí název je <u>FlightDelay</u>.</span><span class="sxs-lookup"><span data-stu-id="499d3-296">hello default name is <u>FlightDelay</u>.</span></span> <span data-ttu-id="499d3-297">Pokud chcete, můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="499d3-297">You can rename it if you want.</span></span>
   * <span data-ttu-id="499d3-298">**$sqlDatabaseMaxSizeGB** – tato hodnota se používá jenom v případě, že vytváříte nový server databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="499d3-298">**$sqlDatabaseMaxSizeGB** - This value is used only when you're creating a new Azure SQL database server.</span></span> <span data-ttu-id="499d3-299">Hello výchozí hodnota je 10GB.</span><span class="sxs-lookup"><span data-stu-id="499d3-299">hello default value is 10GB.</span></span> <span data-ttu-id="499d3-300">10GB je dostačující pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="499d3-300">10GB is sufficient for this tutorial.</span></span>
   * <span data-ttu-id="499d3-301">**$sqlDatabaseName** – tato hodnota se používá jenom v případě, že vytváříte novou databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="499d3-301">**$sqlDatabaseName** - This value is used only when you're creating a new Azure SQL database.</span></span> <span data-ttu-id="499d3-302">Hello výchozí hodnota je HDISqoop.</span><span class="sxs-lookup"><span data-stu-id="499d3-302">hello default value is HDISqoop.</span></span> <span data-ttu-id="499d3-303">Pokud přejmenujete, je nutné aktualizovat skriptu prostředí Windows PowerShell Sqoop hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="499d3-303">If you rename it, you must update hello Sqoop Windows PowerShell script accordingly.</span></span>
4. <span data-ttu-id="499d3-304">Stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="499d3-304">Press **F5** toorun hello script.</span></span>
5. <span data-ttu-id="499d3-305">Ověření výstupu skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="499d3-305">Validate hello script output.</span></span> <span data-ttu-id="499d3-306">Ujistěte se, že skript hello proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="499d3-306">Make sure hello script ran successfully.</span></span>

## <span data-ttu-id="499d3-307"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="499d3-307"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="499d3-308">Nyní víte, jak tooupload soubor tooAzure úložiště objektů Blob, jak toopopulate podregistru tabulky pomocí hello dat z úložiště objektů Blob v Azure, jak toorun Hive dotazy a jak toouse Sqoop tooexport data z HDFS tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="499d3-308">Now you understand how tooupload a file tooAzure Blob storage, how toopopulate a Hive table by using hello data from Azure Blob storage, how toorun Hive queries, and how toouse Sqoop tooexport data from HDFS tooan Azure SQL database.</span></span> <span data-ttu-id="499d3-309">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="499d3-309">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="499d3-310">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="499d3-310">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="499d3-311">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="499d3-311">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="499d3-312">[Použijte Oozie s HDInsight][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="499d3-312">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="499d3-313">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="499d3-313">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="499d3-314">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="499d3-314">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="499d3-315">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="499d3-315">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

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
