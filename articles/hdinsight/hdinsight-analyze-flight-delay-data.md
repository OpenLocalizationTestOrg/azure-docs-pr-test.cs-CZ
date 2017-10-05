---
title: "Analýza dat zpoždění letu s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Naučte se používat jeden skript prostředí Windows PowerShell k vytvoření clusteru HDInsight, spouštět úlohy Hive, Sqoop úlohu spusťte a odstranění clusteru."
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
ms.openlocfilehash: 77790136c9bd3a4e3f7dcabea2fbe0bcffb6eafe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a><span data-ttu-id="acdb6-103">Analýza dat zpoždění letu pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="acdb6-103">Analyze flight delay data by using Hive in HDInsight</span></span>
<span data-ttu-id="acdb6-104">Hive zajišťuje spuštěných úloh Hadoop MapReduce prostřednictvím SQL jako skriptovacího jazyka nazvaného  *[HiveQL][hadoop-hiveql]*, který je možné použít ke shrnutí, dotazování, a analýze velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="acdb6-104">Hive provides a means of running Hadoop MapReduce jobs through an SQL-like scripting language called *[HiveQL][hadoop-hiveql]*, which can be applied towards summarizing, querying, and analyzing large volumes of data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="acdb6-105">Kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="acdb6-105">The steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="acdb6-106">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="acdb6-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="acdb6-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="acdb6-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="acdb6-108">Kroky, které pracují s clusterem se systémem Linux najdete v tématu [analyzovat data zpoždění letu pomocí Hive v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="acdb6-108">For steps that work with a Linux-based cluster, see [Analyze flight delay data by using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).</span></span>

<span data-ttu-id="acdb6-109">Jeden z největších výhod Azure HDInsight je oddělení dat úložiště a výpočty.</span><span class="sxs-lookup"><span data-stu-id="acdb6-109">One of the major benefits of Azure HDInsight is the separation of data storage and compute.</span></span> <span data-ttu-id="acdb6-110">HDInsight používá úložiště objektů Blob v Azure pro úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="acdb6-110">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="acdb6-111">Typické úlohy sestává ze tří částí:</span><span class="sxs-lookup"><span data-stu-id="acdb6-111">A typical job involves three parts:</span></span>

1. <span data-ttu-id="acdb6-112">**Ukládání dat v úložišti objektů Blob Azure.**</span><span class="sxs-lookup"><span data-stu-id="acdb6-112">**Store data in Azure Blob storage.**</span></span>  <span data-ttu-id="acdb6-113">Například informace o počasí dat, data snímačů, webových protokolů a v takovém případě letu zpoždění data se uloží do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-113">For example, weather data, sensor data, web logs, and in this case, flight delay data are saved into Azure Blob storage.</span></span>
2. <span data-ttu-id="acdb6-114">**Spuštění úlohy.**</span><span class="sxs-lookup"><span data-stu-id="acdb6-114">**Run jobs.**</span></span> <span data-ttu-id="acdb6-115">Pokud bude čas zpracování dat, spusťte skript prostředí Windows PowerShell (nebo klientské aplikace) k vytvoření clusteru HDInsight, spouštění úloh a odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="acdb6-115">When it is time to process the data, you run a Windows PowerShell script (or a client application) to create an HDInsight cluster, run jobs, and delete the cluster.</span></span> <span data-ttu-id="acdb6-116">Úlohy uložit výstupní data do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-116">The jobs save output data to Azure Blob storage.</span></span> <span data-ttu-id="acdb6-117">Výstupní data se uchovávají i po odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="acdb6-117">The output data is retained even after the cluster is deleted.</span></span> <span data-ttu-id="acdb6-118">Tímto způsobem platíte jenom to, na co mají použít.</span><span class="sxs-lookup"><span data-stu-id="acdb6-118">This way, you pay for only what you have consumed.</span></span>
3. <span data-ttu-id="acdb6-119">**Načíst výstup z Azure Blob storage**, nebo v tomto kurzu exportovat data do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="acdb6-119">**Retrieve the output from Azure Blob storage**, or in this tutorial, export the data to an Azure SQL database.</span></span>

<span data-ttu-id="acdb6-120">Následující diagram znázorňuje tento scénář a struktura v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="acdb6-120">The following diagram illustrates the scenario and the structure of this tutorial:</span></span>

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

<span data-ttu-id="acdb6-122">Všimněte si, že čísla v diagramu odpovídají názvů oddílů.</span><span class="sxs-lookup"><span data-stu-id="acdb6-122">Note that the numbers in the diagram correspond to the section titles.</span></span> <span data-ttu-id="acdb6-123">**M** znamená hlavní proces.</span><span class="sxs-lookup"><span data-stu-id="acdb6-123">**M** stands for the main process.</span></span> <span data-ttu-id="acdb6-124">**A** znamená pro obsah v příloze.</span><span class="sxs-lookup"><span data-stu-id="acdb6-124">**A** stands for the content in the appendix.</span></span>

<span data-ttu-id="acdb6-125">Hlavní část kurzu se dozvíte, jak používat jeden skript prostředí Windows PowerShell k provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="acdb6-125">The main portion of the tutorial shows you how to use one Windows PowerShell script to perform the following tasks:</span></span>

* <span data-ttu-id="acdb6-126">Vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="acdb6-126">Create an HDInsight cluster.</span></span>
* <span data-ttu-id="acdb6-127">Spuštění úlohy Hive v clusteru pro výpočet průměrné zpoždění na letištích.</span><span class="sxs-lookup"><span data-stu-id="acdb6-127">Run a Hive job on the cluster to calculate average delays at airports.</span></span> <span data-ttu-id="acdb6-128">Data pohybující se zpoždění uložený v účtu úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-128">The flight delay data is stored in an Azure Blob storage account.</span></span>
* <span data-ttu-id="acdb6-129">Spustíte úlohu Sqoop výstup úlohy Hive export do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="acdb6-129">Run a Sqoop job to export the Hive job output to an Azure SQL database.</span></span>
* <span data-ttu-id="acdb6-130">Odstranění clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="acdb6-130">Delete the HDInsight cluster.</span></span>

<span data-ttu-id="acdb6-131">V dodatky najdete pokyny k nahrávání letu zpoždění dat, vytváření nebo odeslání řetězec dotazu Hive a příprava úlohy Sqoop Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="acdb6-131">In the appendixes, you can find the instructions for uploading flight delay data, creating/uploading a Hive query string, and preparing the Azure SQL database for the Sqoop job.</span></span>

> [!NOTE]
> <span data-ttu-id="acdb6-132">Kroky v tomto dokumentu jsou specifická pro clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="acdb6-132">The steps in this document are specific to Windows-based HDInsight clusters.</span></span> <span data-ttu-id="acdb6-133">Kroky, které pracují s clusterem se systémem Linux najdete v tématu [analyzovat data zpoždění letu používání Hive v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span><span class="sxs-lookup"><span data-stu-id="acdb6-133">For steps that work with a Linux-based cluster, see [Analyze flight delay data using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span></span>

### <a name="prerequisites"></a><span data-ttu-id="acdb6-134">Požadavky</span><span class="sxs-lookup"><span data-stu-id="acdb6-134">Prerequisites</span></span>
<span data-ttu-id="acdb6-135">Před zahájením tohoto kurzu musíte mít tyto položky:</span><span class="sxs-lookup"><span data-stu-id="acdb6-135">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="acdb6-136">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="acdb6-136">**An Azure subscription**.</span></span> <span data-ttu-id="acdb6-137">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="acdb6-137">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="acdb6-138">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="acdb6-138">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="acdb6-139">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="acdb6-139">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="acdb6-140">Kroky v tomto dokumentu používají nové rutiny služby HDInsight, které pracují s Azure Resource Managerem.</span><span class="sxs-lookup"><span data-stu-id="acdb6-140">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="acdb6-141">Podle postupu v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) si nainstalujte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="acdb6-141">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="acdb6-142">Pokud máte skripty, které je potřeba upravit tak, aby používaly nové rutiny, které pracují s nástrojem Azure Resource Manager, najdete další informace v tématu [Migrace na vývojové nástroje založené na Azure Resource Manageru pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="acdb6-142">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

<span data-ttu-id="acdb6-143">**Soubory používané v tomto kurzu**</span><span class="sxs-lookup"><span data-stu-id="acdb6-143">**Files used in this tutorial**</span></span>

<span data-ttu-id="acdb6-144">Tento kurz používá na časový výkon letecká společnost letu data z [výzkum a inovativní technologie správy, úřad Transport statistických nebo RITĚ][rita-website].</span><span class="sxs-lookup"><span data-stu-id="acdb6-144">This tutorial uses the on-time performance of airline flight data from [Research and Innovative Technology Administration, Bureau of Transportation Statistics or RITA][rita-website].</span></span>
<span data-ttu-id="acdb6-145">Kopírování dat byl odeslán do kontejner úložiště objektů Blob v Azure s oprávněním přístup veřejného objektu Blob.</span><span class="sxs-lookup"><span data-stu-id="acdb6-145">A copy of the data has been uploaded to an Azure Blob storage container with the Public Blob access permission.</span></span>
<span data-ttu-id="acdb6-146">Součástí vašeho skriptu prostředí PowerShell zkopíruje data z kontejneru veřejného objektu blob na výchozí kontejner objektu blob ve vašem clusteru.</span><span class="sxs-lookup"><span data-stu-id="acdb6-146">A part of your PowerShell script copies the data from the public blob container to the default blob container of your cluster.</span></span> <span data-ttu-id="acdb6-147">Skript HiveQL je také zkopírován do stejné kontejneru objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="acdb6-147">The HiveQL script is also copied to the same Blob container.</span></span>
<span data-ttu-id="acdb6-148">Pokud chcete zjistit, jak get/nahrávání dat do účtu úložiště a jak vytvořit nebo nahráváte soubor skriptu HiveQL, najdete v článku [příloha A](#appendix-a) a [příloha B](#appendix-b).</span><span class="sxs-lookup"><span data-stu-id="acdb6-148">If you want to learn how to get/upload the data to your own Storage account, and how to create/upload the HiveQL script file, see [Appendix A](#appendix-a) and [Appendix B](#appendix-b).</span></span>

<span data-ttu-id="acdb6-149">Následující tabulka uvádí soubory používané v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="acdb6-149">The following table lists the files used in this tutorial:</span></span>

<table border="1">
<tr><th><span data-ttu-id="acdb6-150">Soubory</span><span class="sxs-lookup"><span data-stu-id="acdb6-150">Files</span></span></th><th><span data-ttu-id="acdb6-151">Popis</span><span class="sxs-lookup"><span data-stu-id="acdb6-151">Description</span></span></th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td><span data-ttu-id="acdb6-152">Soubor skriptu HiveQL používané úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="acdb6-152">The HiveQL script file used by the Hive job.</span></span> <span data-ttu-id="acdb6-153">Tento skript byl odeslán do účtu úložiště objektů Blob v Azure s veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="acdb6-153">This script has been uploaded to an Azure Blob storage account with the public access.</span></span> <span data-ttu-id="acdb6-154"><a href="#appendix-b">Dodatek B</a> obsahuje pokyny k přípravě a pak tento soubor nahrát na svůj vlastní účet úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-154"><a href="#appendix-b">Appendix B</a> has instructions on preparing and uploading this file to your own Azure Blob storage account.</span></span></td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td><span data-ttu-id="acdb6-155">Vstupní data pro úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="acdb6-155">Input data for the Hive job.</span></span> <span data-ttu-id="acdb6-156">Data byla uložena do účtu úložiště objektů Blob v Azure s veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="acdb6-156">The data has been uploaded to an Azure Blob storage account with the public access.</span></span> <span data-ttu-id="acdb6-157"><a href="#appendix-a">Příloha A</a> obsahuje informace o získání dat a odesílání dat na svůj vlastní účet úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-157"><a href="#appendix-a">Appendix A</a> has instructions on getting the data and uploading the data to your own Azure Blob storage account.</span></span></td></tr>
<tr><td><span data-ttu-id="acdb6-158">\tutorials\flightdelays\output</span><span class="sxs-lookup"><span data-stu-id="acdb6-158">\tutorials\flightdelays\output</span></span></td><td><span data-ttu-id="acdb6-159">Výstupní cesta pro úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="acdb6-159">The output path for the Hive job.</span></span> <span data-ttu-id="acdb6-160">Výchozí kontejner se používá pro ukládání výstupní data.</span><span class="sxs-lookup"><span data-stu-id="acdb6-160">The default container is used for storing the output data.</span></span></td></tr>
<tr><td><span data-ttu-id="acdb6-161">\tutorials\flightdelays\jobstatus</span><span class="sxs-lookup"><span data-stu-id="acdb6-161">\tutorials\flightdelays\jobstatus</span></span></td><td><span data-ttu-id="acdb6-162">Hive složka stav úlohy na výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="acdb6-162">The Hive job status folder on the default container.</span></span></td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a><span data-ttu-id="acdb6-163">Vytvoření clusteru a spouštění úloh Hive nebo Sqoop</span><span class="sxs-lookup"><span data-stu-id="acdb6-163">Create cluster and run Hive/Sqoop jobs</span></span>
<span data-ttu-id="acdb6-164">Hadoop MapReduce je dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="acdb6-164">Hadoop MapReduce is batch processing.</span></span> <span data-ttu-id="acdb6-165">Nákladově nejefektivnější způsob spuštění úlohy Hive je vytvoření clusteru s podporou pro úlohu a odstranit tuto úlohu po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="acdb6-165">The most cost-effective way to run a Hive job is to create a cluster for the job, and delete the job after the job is completed.</span></span> <span data-ttu-id="acdb6-166">Následující skript obsahuje celého procesu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-166">The following script covers the whole process.</span></span>
<span data-ttu-id="acdb6-167">Další informace o vytváření clusteru služby HDInsight a spuštění úloh Hive naleznete v tématu [vytvoření Hadoop clusterů v HDInsight] [ hdinsight-provision] a [používání Hive s HDInsight] [hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="acdb6-167">For more information on creating an HDInsight cluster and running Hive jobs, see [Create Hadoop clusters in HDInsight][hdinsight-provision] and [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

<span data-ttu-id="acdb6-168">**Ke spouštění dotazů Hive pomocí prostředí Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="acdb6-168">**To run the Hive queries by Azure PowerShell**</span></span>

1. <span data-ttu-id="acdb6-169">Vytvoření databáze Azure SQL a v tabulce pro výstup úlohy Sqoop pomocí pokynů uvedených v [příloha C](#appendix-c).</span><span class="sxs-lookup"><span data-stu-id="acdb6-169">Create an Azure SQL database and the table for the Sqoop job output by using the instructions in [Appendix C](#appendix-c).</span></span>
2. <span data-ttu-id="acdb6-170">Otevřete Windows PowerShell ISE a spusťte následující skript:</span><span class="sxs-lookup"><span data-stu-id="acdb6-170">Open Windows PowerShell ISE, and run the following script:</span></span>

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure the follwing variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different

    ###########################################
    # (Optional) configure the following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter the Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use the cluster name

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

    # Create the default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create the HDInsight cluster
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
    # Prepare the HiveQL script and source data
    ###########################################

    # Create the temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data to default container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit the Hive job
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
    # Submit the Sqoop job
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
    # Delete the cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. <span data-ttu-id="acdb6-171">Připojení k vaší databázi SQL a najdete v části zpoždění letů průměrná podle města v tabulce AvgDelays:</span><span class="sxs-lookup"><span data-stu-id="acdb6-171">Connect to your SQL database and see average flight delays by city in the AvgDelays table:</span></span>

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <span data-ttu-id="acdb6-173"><a id="appendix-a"></a>Příloha A - nahrávání letu zpoždění data do úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="acdb6-173"><a id="appendix-a"></a>Appendix A - Upload flight delay data to Azure Blob storage</span></span>
<span data-ttu-id="acdb6-174">Odesílání datového souboru a soubory skript HiveQL (viz [příloha B](#appendix-b)) vyžaduje některé plánování.</span><span class="sxs-lookup"><span data-stu-id="acdb6-174">Uploading the data file and the HiveQL script files (see [Appendix B](#appendix-b)) requires some planning.</span></span> <span data-ttu-id="acdb6-175">Cílem je, a ukládat soubory dat a soubor HiveQL předtím, než vytváření clusteru služby HDInsight a spuštění úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="acdb6-175">The idea is to store the data files and the HiveQL file before creating an HDInsight cluster and running the Hive job.</span></span> <span data-ttu-id="acdb6-176">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="acdb6-176">You have two options:</span></span>

* <span data-ttu-id="acdb6-177">**Použijte stejný účet úložiště Azure, který se použije jako výchozí systém souborů v clusteru HDInsight.**</span><span class="sxs-lookup"><span data-stu-id="acdb6-177">**Use the same Azure Storage account that will be used by the HDInsight cluster as the default file system.**</span></span> <span data-ttu-id="acdb6-178">Protože přístupový klíč účtu úložiště bude mít HDInsight cluster, nemusíte provádět žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="acdb6-178">Because the HDInsight cluster will have the Storage account access key, you don't need to make any additional changes.</span></span>
* <span data-ttu-id="acdb6-179">**Použijte jiný účet úložiště Azure z clusteru HDInsight výchozí systém souborů.**</span><span class="sxs-lookup"><span data-stu-id="acdb6-179">**Use a different Azure Storage account from the HDInsight cluster default file system.**</span></span> <span data-ttu-id="acdb6-180">Pokud je to tento případ, musíte upravit vytvoření součástí prostředí Windows PowerShell skriptu v nalezen [clusteru HDInsight se vytvoření a spuštění úlohy Hive nebo Sqoop](#runjob) propojení účtu úložiště jako další účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="acdb6-180">If this is the case, you must modify the creation part of the Windows PowerShell script found in [Create HDInsight cluster and run Hive/Sqoop jobs](#runjob) to link the Storage account as an additional Storage account.</span></span> <span data-ttu-id="acdb6-181">Pokyny najdete v tématu [vytvoření Hadoop clusterů v HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="acdb6-181">For instructions, see [Create Hadoop clusters in HDInsight][hdinsight-provision].</span></span> <span data-ttu-id="acdb6-182">HDInsight cluster pak zná přístupový klíč pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="acdb6-182">The HDInsight cluster then knows the access key for the Storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="acdb6-183">Cesta k úložišti objektů Blob pro datový soubor je pevný programového v souboru skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-183">The Blob storage path for the data file is hard coded in the HiveQL script file.</span></span> <span data-ttu-id="acdb6-184">Je třeba jej aktualizovat odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="acdb6-184">You must update it accordingly.</span></span>

<span data-ttu-id="acdb6-185">**Ke stahování dat letu**</span><span class="sxs-lookup"><span data-stu-id="acdb6-185">**To download the flight data**</span></span>

1. <span data-ttu-id="acdb6-186">Přejděte do [výzkum a inovativní technologie správy, statistický úřad Transport][rita-website].</span><span class="sxs-lookup"><span data-stu-id="acdb6-186">Browse to [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>
2. <span data-ttu-id="acdb6-187">Na stránce vyberte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="acdb6-187">On the page, select the following values:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="acdb6-188">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="acdb6-188">Name</span></span></th><th><span data-ttu-id="acdb6-189">Hodnota</span><span class="sxs-lookup"><span data-stu-id="acdb6-189">Value</span></span></th></tr>
    <tr><td><span data-ttu-id="acdb6-190">Filtr roku</span><span class="sxs-lookup"><span data-stu-id="acdb6-190">Filter Year</span></span></td><td><span data-ttu-id="acdb6-191">2013</span><span class="sxs-lookup"><span data-stu-id="acdb6-191">2013</span></span> </td></tr>
    <tr><td><span data-ttu-id="acdb6-192">Filtrovat období</span><span class="sxs-lookup"><span data-stu-id="acdb6-192">Filter Period</span></span></td><td><span data-ttu-id="acdb6-193">Leden</span><span class="sxs-lookup"><span data-stu-id="acdb6-193">January</span></span></td></tr>
    <tr><td><span data-ttu-id="acdb6-194">Pole</span><span class="sxs-lookup"><span data-stu-id="acdb6-194">Fields</span></span></td><td><span data-ttu-id="acdb6-195">*Rok*, *FlightDate*, *UniqueCarrier*, *poskytovatel*, *FlightNum*, *OriginAirportID* , *Původu*, *OriginCityName*, *OriginState*, *DestAirportID*, *cíle* , *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (zrušte zaškrtnutí všech ostatních polí)</span><span class="sxs-lookup"><span data-stu-id="acdb6-195">*Year*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (clear all other fields)</span></span></td></tr>
    </table><span data-ttu-id="acdb6-196">
3.Klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="acdb6-196">
3. Click **Download**.</span></span>
<span data-ttu-id="acdb6-197">4.</span><span class="sxs-lookup"><span data-stu-id="acdb6-197">4.</span></span> <span data-ttu-id="acdb6-198">Rozbalte soubor **C:\Tutorials\FlightDelay\2013Data** složky.</span><span class="sxs-lookup"><span data-stu-id="acdb6-198">Unzip the file to the **C:\Tutorials\FlightDelay\2013Data** folder.</span></span> <span data-ttu-id="acdb6-199">Každý soubor je soubor CSV a je přibližně 60GB.</span><span class="sxs-lookup"><span data-stu-id="acdb6-199">Each file is a CSV file and is approximately 60GB in size.</span></span>
<span data-ttu-id="acdb6-200">5.</span><span class="sxs-lookup"><span data-stu-id="acdb6-200">5.</span></span> <span data-ttu-id="acdb6-201">Přejmenujte soubor na název v měsíci, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="acdb6-201">Rename the file to the name of the month that it contains data for.</span></span> <span data-ttu-id="acdb6-202">Například by s názvem souboru, který obsahuje data leden *January.csv*.</span><span class="sxs-lookup"><span data-stu-id="acdb6-202">For example, the file containing the January data would be named *January.csv*.</span></span>
<span data-ttu-id="acdb6-203">6.</span><span class="sxs-lookup"><span data-stu-id="acdb6-203">6.</span></span> <span data-ttu-id="acdb6-204">Opakujte kroky 2 a 5 na stažení souboru pro každou dobu 12 měsíců v 2013.</span><span class="sxs-lookup"><span data-stu-id="acdb6-204">Repeat steps 2 and 5 to download a file for each of the 12 months in 2013.</span></span> <span data-ttu-id="acdb6-205">Budete potřebovat minimálně jeden soubor ke spuštění tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-205">You will need a minimum of one file to run the tutorial.</span></span>

<span data-ttu-id="acdb6-206">**Odeslání dat zpoždění letu do úložiště objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="acdb6-206">**To upload the flight delay data to Azure Blob storage**</span></span>

1. <span data-ttu-id="acdb6-207">Připravte parametry:</span><span class="sxs-lookup"><span data-stu-id="acdb6-207">Prepare the parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="acdb6-208">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="acdb6-208">Variable Name</span></span></th><th><span data-ttu-id="acdb6-209">Poznámky</span><span class="sxs-lookup"><span data-stu-id="acdb6-209">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="acdb6-210">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="acdb6-210">$storageAccountName</span></span></td><td><span data-ttu-id="acdb6-211">Kam chcete nahrát data do účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-211">The Azure Storage account where you want to upload the data to.</span></span></td></tr>
    <tr><td><span data-ttu-id="acdb6-212">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="acdb6-212">$blobContainerName</span></span></td><td><span data-ttu-id="acdb6-213">Kam chcete nahrát data do kontejneru objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="acdb6-213">The Blob container where you want to upload the data to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="acdb6-214">Otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="acdb6-214">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="acdb6-215">3.</span><span class="sxs-lookup"><span data-stu-id="acdb6-215">3.</span></span> <span data-ttu-id="acdb6-216">Vložte následující skript do podokna skriptu:</span><span class="sxs-lookup"><span data-stu-id="acdb6-216">Paste the following script into the script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
    $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
    #EndRegion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy the file from local workstation to Azure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName to $blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
    }

    # List the uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. <span data-ttu-id="acdb6-217">Stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="acdb6-217">Press **F5** to run the script.</span></span>

<span data-ttu-id="acdb6-218">Pokud chcete použít jinou metodu pro nahrávání souborů, Zkontrolujte prosím, že je cesta k souboru kurzy/flightdelay nebo data.</span><span class="sxs-lookup"><span data-stu-id="acdb6-218">If you choose to use a different method for uploading the files, please make sure the file path is tutorials/flightdelay/data.</span></span> <span data-ttu-id="acdb6-219">Syntaxe pro přístup k souborům je:</span><span class="sxs-lookup"><span data-stu-id="acdb6-219">The syntax for accessing the files is:</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

<span data-ttu-id="acdb6-220">Cesta flightdelay/kurzy nebo dat je virtuální složky, kterou jste vytvořili, když jste odeslali soubory.</span><span class="sxs-lookup"><span data-stu-id="acdb6-220">The path tutorials/flightdelay/data is the virtual folder you created when you uploaded the files.</span></span> <span data-ttu-id="acdb6-221">Ověřte, jestli jsou 12 soubory, jeden pro každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="acdb6-221">Verify that there are 12 files, one for each month.</span></span>

> [!NOTE]
> <span data-ttu-id="acdb6-222">Je třeba aktualizovat dotaz Hive ke čtení z nového umístění.</span><span class="sxs-lookup"><span data-stu-id="acdb6-222">You must update the Hive query to read from the new location.</span></span>
>
> <span data-ttu-id="acdb6-223">Buď je nutné nakonfigurovat oprávnění ke kontejneru přístupu veřejné nebo vytvořit vazbu na účet úložiště na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="acdb6-223">You must either configure the container access permission to be public or bind the Storage account to the HDInsight cluster.</span></span> <span data-ttu-id="acdb6-224">Řetězec dotazu Hive, jinak nebudou mít přístup k datové soubory.</span><span class="sxs-lookup"><span data-stu-id="acdb6-224">Otherwise, the Hive query string will not be able to access the data files.</span></span>

- - -

## <span data-ttu-id="acdb6-225"><a id="appendix-b"></a>Dodatek B – vytvořit a odeslat skript HiveQL</span><span class="sxs-lookup"><span data-stu-id="acdb6-225"><a id="appendix-b"></a>Appendix B - Create and upload a HiveQL script</span></span>
<span data-ttu-id="acdb6-226">Pomocí Azure PowerShell, můžete současně spustit více příkazy HiveQL jeden nebo balíček příkaz HiveQL do souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-226">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package the HiveQL statement into a script file.</span></span> <span data-ttu-id="acdb6-227">V této části se dozvíte, jak vytvořit skript HiveQL a odeslat skript do úložiště objektů Blob v Azure pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="acdb6-227">This section shows you how to create a HiveQL script and upload the script to Azure Blob storage by using Azure PowerShell.</span></span> <span data-ttu-id="acdb6-228">Hive vyžaduje HiveQL skriptů k uložení do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-228">Hive requires the HiveQL scripts to be stored in Azure Blob storage.</span></span>

<span data-ttu-id="acdb6-229">Skript HiveQL provede následující:</span><span class="sxs-lookup"><span data-stu-id="acdb6-229">The HiveQL script will perform the following:</span></span>

1. <span data-ttu-id="acdb6-230">**Odpojit tabulku delays_raw**, v případě, že v tabulce již existuje.</span><span class="sxs-lookup"><span data-stu-id="acdb6-230">**Drop the delays_raw table**, in case the table already exists.</span></span>
2. <span data-ttu-id="acdb6-231">**Vytvoří externí tabulku Hive delays_raw** odkazující na umístění úložiště objektů Blob v souborech zpoždění letu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-231">**Create the delays_raw external Hive table** pointing to the Blob storage location with the flight delay files.</span></span> <span data-ttu-id="acdb6-232">Tento dotaz Určuje, že pole jsou oddělená "," a že řádky se ukončila příkazem "\n".</span><span class="sxs-lookup"><span data-stu-id="acdb6-232">This query specifies that fields are delimited by "," and that lines are terminated by "\n".</span></span> <span data-ttu-id="acdb6-233">To představuje problém, když hodnoty polí obsahovat čárky, protože podregistr nelze rozlišit mezi čárkami, který je oddělovačem polí a ten, který je součástí hodnotu pole (což je případ hodnoty v polích pro POČÁTEK\_MĚSTA\_název a cíl\_ MĚSTA\_název).</span><span class="sxs-lookup"><span data-stu-id="acdb6-233">This poses a problem when field values contain commas because Hive cannot differentiate between a comma that is a field delimiter and a one that is part of a field value (which is the case in field values for ORIGIN\_CITY\_NAME and DEST\_CITY\_NAME).</span></span> <span data-ttu-id="acdb6-234">Chcete-li to vyřešit, vytvoří dotaz dočasné sloupce k ukládání dat, která je nesprávně rozdělená do sloupce.</span><span class="sxs-lookup"><span data-stu-id="acdb6-234">To address this, the query creates TEMP columns to hold data that is incorrectly split into columns.</span></span>
3. <span data-ttu-id="acdb6-235">**Odpojit tabulku zpoždění**, v případě, že v tabulce již existuje.</span><span class="sxs-lookup"><span data-stu-id="acdb6-235">**Drop the delays table**, in case the table already exists.</span></span>
4. <span data-ttu-id="acdb6-236">**Vytvoření tabulky zpoždění**.</span><span class="sxs-lookup"><span data-stu-id="acdb6-236">**Create the delays table**.</span></span> <span data-ttu-id="acdb6-237">Je vhodné vyčistit data před další zpracování.</span><span class="sxs-lookup"><span data-stu-id="acdb6-237">It is helpful to clean up the data before further processing.</span></span> <span data-ttu-id="acdb6-238">Tento dotaz vytvoří novou tabulku, *zpoždění*, z tabulky delays_raw.</span><span class="sxs-lookup"><span data-stu-id="acdb6-238">This query creates a new table, *delays*, from the delays_raw table.</span></span> <span data-ttu-id="acdb6-239">Všimněte si, že nejsou zkopírovány dočasné sloupce (jak je uvedeno nahoře) a že **substring** funkce slouží k odebrání dat uvozovky.</span><span class="sxs-lookup"><span data-stu-id="acdb6-239">Note that the TEMP columns (as mentioned previously) are not copied, and that the **substring** function is used to remove quotation marks from the data.</span></span>
5. <span data-ttu-id="acdb6-240">**Výpočetní průměrná počasí zpoždění a skupiny výsledky podle název města.**</span><span class="sxs-lookup"><span data-stu-id="acdb6-240">**Compute the average weather delay and groups the results by city name.**</span></span> <span data-ttu-id="acdb6-241">Je také výstup výsledků do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="acdb6-241">It will also output the results to Blob storage.</span></span> <span data-ttu-id="acdb6-242">Poznámka: aby dotaz dojde k odebrání apostrofy z dat a vyloučí řádků, kde hodnota **weather_delay** má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="acdb6-242">Note that the query will remove apostrophes from the data and will exclude rows where the value for **weather_delay** is null.</span></span> <span data-ttu-id="acdb6-243">To je nezbytné, protože Sqoop, použít později v tomto kurzu, nemůže pracovat s těmito hodnotami řádně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="acdb6-243">This is necessary because Sqoop, used later in this tutorial, doesn't handle those values gracefully by default.</span></span>

<span data-ttu-id="acdb6-244">Úplný seznam příkazy HiveQL, najdete v části [Hive Data Definition Language][hadoop-hiveql].</span><span class="sxs-lookup"><span data-stu-id="acdb6-244">For a full list of the HiveQL commands, see [Hive Data Definition Language][hadoop-hiveql].</span></span> <span data-ttu-id="acdb6-245">Každý příkaz HiveQL musí ukončit středníkem.</span><span class="sxs-lookup"><span data-stu-id="acdb6-245">Each HiveQL command must terminate with a semicolon.</span></span>

<span data-ttu-id="acdb6-246">**K vytvoření souboru skriptu HiveQL**</span><span class="sxs-lookup"><span data-stu-id="acdb6-246">**To create a HiveQL script file**</span></span>

1. <span data-ttu-id="acdb6-247">Připravte parametry:</span><span class="sxs-lookup"><span data-stu-id="acdb6-247">Prepare the parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="acdb6-248">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="acdb6-248">Variable Name</span></span></th><th><span data-ttu-id="acdb6-249">Poznámky</span><span class="sxs-lookup"><span data-stu-id="acdb6-249">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="acdb6-250">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="acdb6-250">$storageAccountName</span></span></td><td><span data-ttu-id="acdb6-251">Pokud chcete odeslat skript HiveQL k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-251">The Azure Storage account where you want to upload the HiveQL script to.</span></span></td></tr>
    <tr><td><span data-ttu-id="acdb6-252">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="acdb6-252">$blobContainerName</span></span></td><td><span data-ttu-id="acdb6-253">Kontejner objektů Blob, kde chcete odeslat skript HiveQL k.</span><span class="sxs-lookup"><span data-stu-id="acdb6-253">The Blob container where you want to upload the HiveQL script to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="acdb6-254">Otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="acdb6-254">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="acdb6-255">3.</span><span class="sxs-lookup"><span data-stu-id="acdb6-255">3.</span></span> <span data-ttu-id="acdb6-256">Zkopírujte a vložte následující skript do podokna skriptu:</span><span class="sxs-lookup"><span data-stu-id="acdb6-256">Copy and paste the following script into the script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # The HiveQL script file is exported as this file before it's uploaded to Blob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # The HiveQL script file will be uploaded to Blob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by the HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate the file and file path

    # Check if a file with the same file name already exists on the workstation
    Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create the folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write the Hive script into a local file
    Write-Host "`nWriting the Hive script into a file on your workstation ..." `
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

    #region - Upload the Hive script to the default Blob container
    Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload the file from local workstation to Blob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

    <span data-ttu-id="acdb6-257">Zde jsou proměnné používané ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="acdb6-257">Here are the variables used in the script:</span></span>

   * <span data-ttu-id="acdb6-258">**$hqlLocalFileName** -skript místně uloží soubor skriptu HiveQL před nahráním do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="acdb6-258">**$hqlLocalFileName** - The script saves the HiveQL script file locally before uploading it to Blob storage.</span></span> <span data-ttu-id="acdb6-259">Toto je název souboru.</span><span class="sxs-lookup"><span data-stu-id="acdb6-259">This is the file name.</span></span> <span data-ttu-id="acdb6-260">Výchozí hodnota je <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span><span class="sxs-lookup"><span data-stu-id="acdb6-260">The default value is <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span></span>
   * <span data-ttu-id="acdb6-261">**$hqlBlobName** -Toto je název objektu blob souboru skript HiveQL použít ve službě Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="acdb6-261">**$hqlBlobName** - This is the HiveQL script file blob name used in the Azure Blob storage.</span></span> <span data-ttu-id="acdb6-262">Výchozí hodnota je tutorials/flightdelay/flightdelays.hql.</span><span class="sxs-lookup"><span data-stu-id="acdb6-262">The default value is tutorials/flightdelay/flightdelays.hql.</span></span> <span data-ttu-id="acdb6-263">Protože soubor bude zapisovat přímo do úložiště objektů Azure Blob, není "/" na začátku názvu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="acdb6-263">Because the file will be written directly to Azure Blob storage, there is NOT a "/" at the beginning of the blob name.</span></span> <span data-ttu-id="acdb6-264">Pokud chcete získat přístup k souboru z úložiště objektů Blob, musíte přidat "/" na začátku názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="acdb6-264">If you want to access the file from Blob storage, you will need to add a "/" at the beginning of the file name.</span></span>
   * <span data-ttu-id="acdb6-265">**$srcDataFolder** a **$dstDataFolder** -= "kurzy/flightdelay nebo data" = "kurzy a flightdelay nebo výstupní"</span><span class="sxs-lookup"><span data-stu-id="acdb6-265">**$srcDataFolder** and **$dstDataFolder** - = "tutorials/flightdelay/data" = "tutorials/flightdelay/output"</span></span>

- - -
## <span data-ttu-id="acdb6-266"><a id="appendix-c"></a>Příloha C – Příprava Azure SQL database pro výstup úlohy Sqoop</span><span class="sxs-lookup"><span data-stu-id="acdb6-266"><a id="appendix-c"></a>Appendix C - Prepare an Azure SQL database for the Sqoop job output</span></span>
<span data-ttu-id="acdb6-267">**Příprava databáze SQL (sloučení to pomocí skriptu Sqoop)**</span><span class="sxs-lookup"><span data-stu-id="acdb6-267">**To prepare the SQL database (merge this with the Sqoop script)**</span></span>

1. <span data-ttu-id="acdb6-268">Připravte parametry:</span><span class="sxs-lookup"><span data-stu-id="acdb6-268">Prepare the parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="acdb6-269">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="acdb6-269">Variable Name</span></span></th><th><span data-ttu-id="acdb6-270">Poznámky</span><span class="sxs-lookup"><span data-stu-id="acdb6-270">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="acdb6-271">$sqlDatabaseServerName</span><span class="sxs-lookup"><span data-stu-id="acdb6-271">$sqlDatabaseServerName</span></span></td><td><span data-ttu-id="acdb6-272">Název databáze serveru Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-272">The name of the Azure SQL database server.</span></span> <span data-ttu-id="acdb6-273">Zadejte, co vytvořit nový server.</span><span class="sxs-lookup"><span data-stu-id="acdb6-273">Enter nothing to create a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="acdb6-274">$sqlDatabaseUsername</span><span class="sxs-lookup"><span data-stu-id="acdb6-274">$sqlDatabaseUsername</span></span></td><td><span data-ttu-id="acdb6-275">Přihlašovací jméno pro server databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-275">The login name for the Azure SQL database server.</span></span> <span data-ttu-id="acdb6-276">Pokud $sqlDatabaseServerName stávajícího serveru, se používají přihlašovací jméno a heslo pro přihlášení k ověření serveru.</span><span class="sxs-lookup"><span data-stu-id="acdb6-276">If $sqlDatabaseServerName is an existing server, the login and login password are used to authenticate with the server.</span></span> <span data-ttu-id="acdb6-277">V opačném případě se používají k vytvoření nového serveru.</span><span class="sxs-lookup"><span data-stu-id="acdb6-277">Otherwise they are used to create a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="acdb6-278">$sqlDatabasePassword</span><span class="sxs-lookup"><span data-stu-id="acdb6-278">$sqlDatabasePassword</span></span></td><td><span data-ttu-id="acdb6-279">Heslo pro přihlášení pro server databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-279">The login password for the Azure SQL database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="acdb6-280">$sqlDatabaseLocation</span><span class="sxs-lookup"><span data-stu-id="acdb6-280">$sqlDatabaseLocation</span></span></td><td><span data-ttu-id="acdb6-281">Tato hodnota se používá jenom v případě, že vytváříte nový server databáze Azure.</span><span class="sxs-lookup"><span data-stu-id="acdb6-281">This value is used only when you're creating a new Azure database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="acdb6-282">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="acdb6-282">$sqlDatabaseName</span></span></td><td><span data-ttu-id="acdb6-283">Databázi SQL používanou k vytvoření tabulky AvgDelays Sqoop úlohy.</span><span class="sxs-lookup"><span data-stu-id="acdb6-283">The SQL database used to create the AvgDelays table for the Sqoop job.</span></span> <span data-ttu-id="acdb6-284">Ponechat prázdné vytvoří databázi s názvem HDISqoop.</span><span class="sxs-lookup"><span data-stu-id="acdb6-284">Leaving it blank will create a database called HDISqoop.</span></span> <span data-ttu-id="acdb6-285">Název tabulky pro výstup úlohy Sqoop je AvgDelays.</span><span class="sxs-lookup"><span data-stu-id="acdb6-285">The table name for the Sqoop job output is AvgDelays.</span></span> </td></tr>
    </table>
2. <span data-ttu-id="acdb6-286">Otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="acdb6-286">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="acdb6-287">3.</span><span class="sxs-lookup"><span data-stu-id="acdb6-287">3.</span></span> <span data-ttu-id="acdb6-288">Zkopírujte a vložte následující skript do podokna skriptu:</span><span class="sxs-lookup"><span data-stu-id="acdb6-288">Copy and paste the following script into the script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the region to create the Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
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

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
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

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
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

    #region -  Execute an SQL command to create the AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]
   > <span data-ttu-id="acdb6-289">Tento skript využívá službu representational stavu transfer (REST), http://bot.whatismyipaddress.com, načíst externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-289">The script uses a representational state transfer (REST) service, http://bot.whatismyipaddress.com, to retrieve your external IP address.</span></span> <span data-ttu-id="acdb6-290">IP adresa se používá pro vytvoření pravidla brány firewall pro server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-290">The IP address is used for creating a firewall rule for your SQL database server.</span></span>

    <span data-ttu-id="acdb6-291">Zde jsou některé proměnné používané ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="acdb6-291">Here are some variables used in the script:</span></span>

   * <span data-ttu-id="acdb6-292">**$ipAddressRestService** – výchozí hodnota je http://bot.whatismyipaddress.com.</span><span class="sxs-lookup"><span data-stu-id="acdb6-292">**$ipAddressRestService** - The default value is http://bot.whatismyipaddress.com.</span></span> <span data-ttu-id="acdb6-293">Je veřejnou IP adresu služby REST pro získání externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-293">It is a public IP address REST service for getting your external IP address.</span></span> <span data-ttu-id="acdb6-294">Pokud chcete, můžete použít jiné služby.</span><span class="sxs-lookup"><span data-stu-id="acdb6-294">You can use other services if you want.</span></span> <span data-ttu-id="acdb6-295">Externí IP adresu získat pomocí služby se použije k vytvoření pravidla brány firewall pro server databáze Azure SQL, takže budete mít přístup k databázi z pracovní stanice (pomocí skriptu prostředí Windows PowerShell).</span><span class="sxs-lookup"><span data-stu-id="acdb6-295">The external IP address retrieved through the service will be used to create a firewall rule for your Azure SQL database server, so that you can access the database from your workstation (by using a Windows PowerShell script).</span></span>
   * <span data-ttu-id="acdb6-296">**$fireWallRuleName** -Toto je název pravidla brány firewall pro server databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-296">**$fireWallRuleName** - This is the name of the firewall rule for the Azure SQL database server.</span></span> <span data-ttu-id="acdb6-297">Výchozí název je <u>FlightDelay</u>.</span><span class="sxs-lookup"><span data-stu-id="acdb6-297">The default name is <u>FlightDelay</u>.</span></span> <span data-ttu-id="acdb6-298">Pokud chcete, můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="acdb6-298">You can rename it if you want.</span></span>
   * <span data-ttu-id="acdb6-299">**$sqlDatabaseMaxSizeGB** – tato hodnota se používá jenom v případě, že vytváříte nový server databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-299">**$sqlDatabaseMaxSizeGB** - This value is used only when you're creating a new Azure SQL database server.</span></span> <span data-ttu-id="acdb6-300">Výchozí hodnota je 10GB.</span><span class="sxs-lookup"><span data-stu-id="acdb6-300">The default value is 10GB.</span></span> <span data-ttu-id="acdb6-301">10GB je dostačující pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-301">10GB is sufficient for this tutorial.</span></span>
   * <span data-ttu-id="acdb6-302">**$sqlDatabaseName** – tato hodnota se používá jenom v případě, že vytváříte novou databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="acdb6-302">**$sqlDatabaseName** - This value is used only when you're creating a new Azure SQL database.</span></span> <span data-ttu-id="acdb6-303">Výchozí hodnota je HDISqoop.</span><span class="sxs-lookup"><span data-stu-id="acdb6-303">The default value is HDISqoop.</span></span> <span data-ttu-id="acdb6-304">Pokud přejmenujete, je nutné aktualizovat Sqoop Windows PowerShell skriptu odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="acdb6-304">If you rename it, you must update the Sqoop Windows PowerShell script accordingly.</span></span>
4. <span data-ttu-id="acdb6-305">Stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="acdb6-305">Press **F5** to run the script.</span></span>
5. <span data-ttu-id="acdb6-306">Ověření výstup skriptu.</span><span class="sxs-lookup"><span data-stu-id="acdb6-306">Validate the script output.</span></span> <span data-ttu-id="acdb6-307">Ujistěte se, že skript proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="acdb6-307">Make sure the script ran successfully.</span></span>

## <span data-ttu-id="acdb6-308"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="acdb6-308"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="acdb6-309">Teď víte, jak nahrát soubor do úložiště objektů Blob v Azure, jak naplnit tabulku Hive pomocí dat z Azure Blob storage, jak spouštět dotazy Hive a postup použití nástroje Sqoop exportovat data z HDFS do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="acdb6-309">Now you understand how to upload a file to Azure Blob storage, how to populate a Hive table by using the data from Azure Blob storage, how to run Hive queries, and how to use Sqoop to export data from HDFS to an Azure SQL database.</span></span> <span data-ttu-id="acdb6-310">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="acdb6-310">To learn more, see the following articles:</span></span>

* <span data-ttu-id="acdb6-311">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="acdb6-311">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="acdb6-312">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="acdb6-312">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="acdb6-313">[Použijte Oozie s HDInsight][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="acdb6-313">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="acdb6-314">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="acdb6-314">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="acdb6-315">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="acdb6-315">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="acdb6-316">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="acdb6-316">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

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
