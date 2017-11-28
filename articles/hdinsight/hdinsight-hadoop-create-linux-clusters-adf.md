---
title: "aaaCreate clusterů systému Hadoop na vyžádání pomocí služby Data Factory - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate na vyžádání clusterů systému Hadoop v HDInsight pomocí Azure Data Factory."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: c869776ac270e37dec710b5fc8d2a792d9263129
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a><span data-ttu-id="7c945-103">Vytvářet na vyžádání clusterů systému Hadoop v HDInsight pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7c945-103">Create on-demand Hadoop clusters in HDInsight using Azure Data Factory</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="7c945-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) je služba integrace cloudových dat, která orchestruje a automatizuje hello přesouvání a transformaci dat.</span><span class="sxs-lookup"><span data-stu-id="7c945-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) is a cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="7c945-105">Může vytvořit tooprocess za běhu clusteru HDInsight Hadoop řez vstupní data a odstranění clusteru hello po dokončení zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-105">It can create a HDInsight Hadoop cluster just-in-time tooprocess an input data slice and delete hello cluster when hello processing is complete.</span></span> <span data-ttu-id="7c945-106">Některé z výhod hello pomocí cluster systému HDInsight Hadoop na vyžádání jsou:</span><span class="sxs-lookup"><span data-stu-id="7c945-106">Some of hello benefits of using an on-demand HDInsight Hadoop cluster are:</span></span>

- <span data-ttu-id="7c945-107">Můžete pouze platím pro hello čase úloha běží na clusteru HDInsight Hadoop (plus stručný konfigurovat doby nečinnosti) hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-107">You only pay for hello time job is running on hello HDInsight Hadoop cluster (plus a brief configurable idle time).</span></span> <span data-ttu-id="7c945-108">Hello fakturace pro clustery služby HDInsight se fakturují za minutu, zda jsou jejich používání, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="7c945-108">hello billing for HDInsight clusters is pro-rated per minute, whether you are using them or not.</span></span> <span data-ttu-id="7c945-109">Pokud používáte na vyžádání propojené služby HDInsight v objektu pro vytváření dat, hello clustery jsou vytvořeny na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="7c945-109">When you use an on-demand HDInsight linked service in Data Factory, hello clusters are created on-demand.</span></span> <span data-ttu-id="7c945-110">A po dokončení úlohy hello jsou automaticky odstraněny hello clustery.</span><span class="sxs-lookup"><span data-stu-id="7c945-110">And hello clusters are deleted automatically when hello jobs are completed.</span></span> <span data-ttu-id="7c945-111">Proto platíte jenom pro úlohu hello systémem čas a hello krátké doby nečinnosti (nastavení time to live).</span><span class="sxs-lookup"><span data-stu-id="7c945-111">Therefore, you only pay for hello job running time and hello brief idle time (time-to-live setting).</span></span>
- <span data-ttu-id="7c945-112">Můžete vytvořit pracovní postup pomocí objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="7c945-112">You can create a workflow using a Data Factory pipeline.</span></span> <span data-ttu-id="7c945-113">Například můžete mít hello kanálu toocopy data z tooan systému SQL Server místní úložiště objektů blob v Azure, zpracování dat hello spuštěním skriptu Hive a Pig skript na cluster systému HDInsight Hadoop na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="7c945-113">For example, you can have hello pipeline toocopy data from an on-premises SQL Server tooan Azure blob storage, process hello data by running a Hive script and a Pig script on an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="7c945-114">Zkopírujte hello výsledek data tooan Azure SQL Data Warehouse pro tooconsume BI aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c945-114">Then, copy hello result data tooan Azure SQL Data Warehouse for BI applications tooconsume.</span></span>
- <span data-ttu-id="7c945-115">Můžete naplánovat hello pracovního postupu toorun pravidelně (HODINOVĚ, denně, týdně, měsíčně, atd.).</span><span class="sxs-lookup"><span data-stu-id="7c945-115">You can schedule hello workflow toorun periodically (hourly, daily, weekly, monthly, etc.).</span></span>

<span data-ttu-id="7c945-116">V Azure Data Factory objekt pro vytváření dat může mít jeden nebo více datových kanálů.</span><span class="sxs-lookup"><span data-stu-id="7c945-116">In Azure Data Factory, a data factory can have one or more data pipelines.</span></span> <span data-ttu-id="7c945-117">Datový kanál obsahuje jeden nebo více aktivit.</span><span class="sxs-lookup"><span data-stu-id="7c945-117">A data pipeline has one or more activities.</span></span> <span data-ttu-id="7c945-118">Existují dva typy aktivit: [aktivity přesunu dat](../data-factory/data-factory-data-movement-activities.md) a [aktivit transformace dat](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="7c945-118">There are two types of activities: [Data Movement Activities](../data-factory/data-factory-data-movement-activities.md) and [Data Transformation Activities](../data-factory/data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="7c945-119">Používáte data přesun aktivity (v současné době pouze aktivita kopírování) toomove data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="7c945-119">You use data movement activities (currently, only Copy Activity) toomove data from a source data store tooa destination data store.</span></span> <span data-ttu-id="7c945-120">Data transformace aktivity tootransform nebo zpracování dat použijete.</span><span class="sxs-lookup"><span data-stu-id="7c945-120">You use data transformation activities tootransform/process data.</span></span> <span data-ttu-id="7c945-121">Aktivita HDInsight Hive je jedním z podporovaných službou Data Factory aktivit transformace hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-121">HDInsight Hive Activity is one of hello transformation activities supported by Data Factory.</span></span> <span data-ttu-id="7c945-122">V tomto kurzu použijete aktivitu transformace Hive hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-122">You use hello Hive transformation activity in this tutorial.</span></span>

<span data-ttu-id="7c945-123">Aktivita toouse hive můžete nakonfigurovat vlastní cluster HDInsight Hadoop nebo cluster systému HDInsight Hadoop na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="7c945-123">You can configure a hive activity toouse your own HDInsight Hadoop cluster or an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="7c945-124">V tomto kurzu hello Hive aktivitu v kanálu objekt pro vytváření dat hello je nakonfigurované toouse clusteru HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="7c945-124">In this tutorial, hello Hive activity in hello data factory pipeline is configured toouse an on-demand HDInsight cluster.</span></span> <span data-ttu-id="7c945-125">Proto při hello aktivita spuštěna tooprocess datový řez, stane se toto:</span><span class="sxs-lookup"><span data-stu-id="7c945-125">Therefore, when hello activity runs tooprocess a data slice, here is what happens:</span></span>

1. <span data-ttu-id="7c945-126">Pro můžete za běhu tooprocess hello řez se automaticky vytvoří cluster HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="7c945-126">A HDInsight Hadoop cluster is automatically created for you just-in-time tooprocess hello slice.</span></span>  
2. <span data-ttu-id="7c945-127">spuštění skriptu HiveQL v clusteru hello zpracovává vstupní data Hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-127">hello input data is processed by running a HiveQL script on hello cluster.</span></span>
3. <span data-ttu-id="7c945-128">Po dokončení zpracování hello a hello clusteru hello nakonfigurované množství času (nastavení timeToLive) nečinný, se odstraní Hello clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="7c945-128">hello HDInsight Hadoop cluster is deleted after hello processing is complete and hello cluster is idle for hello configured amount of time (timeToLive setting).</span></span> <span data-ttu-id="7c945-129">Pokud hello další datový řez je k dispozici pro zpracování s timeToLive dobu nečinnosti, hello stejného clusteru je použité tooprocess hello řez.</span><span class="sxs-lookup"><span data-stu-id="7c945-129">If hello next data slice is available for processing with in this timeToLive idle time, hello same cluster is used tooprocess hello slice.</span></span>  

<span data-ttu-id="7c945-130">V tomto kurzu provádí hello skript HiveQL přidružené k aktivitě hive hello hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="7c945-130">In this tutorial, hello HiveQL script associated with hello hive activity performs hello following actions:</span></span>

1. <span data-ttu-id="7c945-131">Vytvoří externí tabulku, která odkazuje na hello nezpracovaná webového protokolu data uložená v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7c945-131">Creates an external table that references hello raw web log data stored in an Azure Blob storage.</span></span>
2. <span data-ttu-id="7c945-132">Hello nezpracovaná data oddíly podle roku a měsíce.</span><span class="sxs-lookup"><span data-stu-id="7c945-132">Partitions hello raw data by year and month.</span></span>
3. <span data-ttu-id="7c945-133">Úložiště hello oddílů dat v hello úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="7c945-133">Stores hello partitioned data in hello Azure blob storage.</span></span>

<span data-ttu-id="7c945-134">V tomto kurzu hello skript HiveQL přidružené k aktivitě hive hello vytvoří externí tabulku, která odkazuje na hello nezpracovaná webového protokolu data uložená v hello Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7c945-134">In this tutorial, hello HiveQL script associated with hello hive activity creates an external table that references hello raw web log data stored in hello Azure Blob Storage.</span></span> <span data-ttu-id="7c945-135">Tady jsou hello ukázka řádků pro každý měsíc ve vstupním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-135">Here are hello sample rows for each month in hello input file.</span></span>

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

<span data-ttu-id="7c945-136">oddíly skript HiveQL Hello hello nezpracovaná data podle roku a měsíce.</span><span class="sxs-lookup"><span data-stu-id="7c945-136">hello HiveQL script partitions hello raw data by year and month.</span></span> <span data-ttu-id="7c945-137">Vytvoří tři výstupní složky podle předchozích vstup hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-137">It creates three output folders based on hello previous input.</span></span> <span data-ttu-id="7c945-138">Daná složka obsahuje soubor s položky z každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="7c945-138">Each folder contains a file with entries from each month.</span></span>

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

<span data-ttu-id="7c945-139">Seznam aktivit transformace dat služby Data Factory v aktivitě tooHive přidání najdete v tématu [transformovat a analyzovat pomocí Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="7c945-139">For a list of Data Factory data transformation activities in addition tooHive activity, see [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7c945-140">V současné době můžete vytvořit pouze verze clusteru HDInsight 3.2 z Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7c945-140">Currently, you can only create HDInsight cluster version 3.2 from Azure Data Factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c945-141">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7c945-141">Prerequisites</span></span>
<span data-ttu-id="7c945-142">Než začnete hello pokyny v tomto článku, musíte mít hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7c945-142">Before you begin hello instructions in this article, you must have hello following items:</span></span>

* <span data-ttu-id="7c945-143">[Předplatné Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="7c945-143">[Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="7c945-144">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="7c945-144">Azure PowerShell.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a><span data-ttu-id="7c945-145">Příprava účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="7c945-145">Prepare storage account</span></span>
<span data-ttu-id="7c945-146">Můžete použít toothree účtů úložiště v tomto scénáři:</span><span class="sxs-lookup"><span data-stu-id="7c945-146">You can use up toothree storage accounts in this scenario:</span></span>

- <span data-ttu-id="7c945-147">Výchozí účet úložiště pro hello HDInsight cluster</span><span class="sxs-lookup"><span data-stu-id="7c945-147">default storage account for hello HDInsight cluster</span></span>
- <span data-ttu-id="7c945-148">účet úložiště pro vstupní data hello</span><span class="sxs-lookup"><span data-stu-id="7c945-148">storage account for hello input data</span></span>
- <span data-ttu-id="7c945-149">účet úložiště hello výstupních dat</span><span class="sxs-lookup"><span data-stu-id="7c945-149">storage account for hello output data</span></span>

<span data-ttu-id="7c945-150">toosimplify hello kurzu použijete jeden úložiště účet tooserve hello tři účely.</span><span class="sxs-lookup"><span data-stu-id="7c945-150">toosimplify hello tutorial, you use one storage account tooserve hello three purposes.</span></span> <span data-ttu-id="7c945-151">najít v této části Hello prostředí Azure PowerShell ukázkový skript provede hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="7c945-151">hello Azure PowerShell sample script found in this section performs hello following tasks:</span></span>

1. <span data-ttu-id="7c945-152">Přihlaste se tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7c945-152">Log in tooAzure.</span></span>
2. <span data-ttu-id="7c945-153">Vytvořte skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7c945-153">Create an Azure resource group.</span></span>
3. <span data-ttu-id="7c945-154">Vytvoření účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7c945-154">Create an Azure Storage account.</span></span>
4. <span data-ttu-id="7c945-155">Vytvořte kontejner objektů Blob v účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="7c945-155">Create a Blob container in hello storage account</span></span>
5. <span data-ttu-id="7c945-156">Zkopírujte následující dva kontejner objektů Blob toohello soubory hello:</span><span class="sxs-lookup"><span data-stu-id="7c945-156">Copy hello following two files toohello Blob container:</span></span>

   * <span data-ttu-id="7c945-157">Vstupní datový soubor: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span><span class="sxs-lookup"><span data-stu-id="7c945-157">Input data file: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span></span>
   * <span data-ttu-id="7c945-158">Skript HiveQL: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span><span class="sxs-lookup"><span data-stu-id="7c945-158">HiveQL script: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span></span>

     <span data-ttu-id="7c945-159">Oba soubory jsou uloženy ve veřejném kontejneru Blob.</span><span class="sxs-lookup"><span data-stu-id="7c945-159">Both files are stored in a public Blob container.</span></span>


<span data-ttu-id="7c945-160">**tooprepare hello úložiště a kopírovat hello soubory pomocí Azure PowerShell:**</span><span class="sxs-lookup"><span data-stu-id="7c945-160">**tooprepare hello storage and copy hello files using Azure PowerShell:**</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7c945-161">Zadejte názvy pro skupinu prostředků Azure hello a hello účtu úložiště Azure, který bude vytvořen skriptem hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-161">Specify names for hello Azure resource group and hello Azure storage account that will be created by hello script.</span></span>
> <span data-ttu-id="7c945-162">Zapište **název skupiny prostředků**, **název účtu úložiště**, a **klíč účtu úložiště** výstupem hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="7c945-162">Write down **resource group name**, **storage account name**, and **storage account key** outputted by hello script.</span></span> <span data-ttu-id="7c945-163">Je nutné v další části hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-163">You need them in hello next section.</span></span>

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect tooAzure
####################################
#region - Connect tooAzure subscription
Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use hello following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

<span data-ttu-id="7c945-164">Pokud potřebujete pomoc s skript prostředí PowerShell text hello, přečtěte si téma [hello pomocí prostředí Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="7c945-164">If you need help with hello PowerShell script, see [Using hello Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span> <span data-ttu-id="7c945-165">Pokud je jako toouse rozhraní příkazového řádku Azure místo toho zobrazí hello [příloha](#appendix) části hello skript příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="7c945-165">If you like toouse Azure CLI instead, see hello [Appendix](#appendix) section for hello Azure CLI script.</span></span>

<span data-ttu-id="7c945-166">**obsah tooexamine hello úložiště účet a hello**</span><span class="sxs-lookup"><span data-stu-id="7c945-166">**tooexamine hello storage account and hello contents**</span></span>

1. <span data-ttu-id="7c945-167">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7c945-167">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7c945-168">Klikněte na tlačítko **skupiny prostředků** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-168">Click **Resource groups** on hello left pane.</span></span>
3. <span data-ttu-id="7c945-169">Dvakrát klikněte na název skupiny prostředků hello, kterou jste vytvořili ve vašem skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7c945-169">Double-click hello resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="7c945-170">Pomocí filtru hello, pokud máte příliš mnoho skupin prostředků, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="7c945-170">Use hello filter if you have too many resource groups listed.</span></span>
4. <span data-ttu-id="7c945-171">Na hello **prostředky** dlaždice, musí mít jeden prostředek uvedené Pokud sdílíte s další projekty skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-171">On hello **Resources** tile, you shall have one resource listed unless you share hello resource group with other projects.</span></span> <span data-ttu-id="7c945-172">Tento prostředek je hello účet úložiště s hello název, který jste zadali dříve.</span><span class="sxs-lookup"><span data-stu-id="7c945-172">That resource is hello storage account with hello name you specified earlier.</span></span> <span data-ttu-id="7c945-173">Klikněte na název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-173">Click hello storage account name.</span></span>
5. <span data-ttu-id="7c945-174">Klikněte na tlačítko hello **objekty BLOB** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="7c945-174">Click hello **Blobs** tiles.</span></span>
6. <span data-ttu-id="7c945-175">Klikněte na tlačítko hello **adfgetstarted** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7c945-175">Click hello **adfgetstarted** container.</span></span> <span data-ttu-id="7c945-176">Zobrazí dvě složky: **inputdata** a **skriptu**.</span><span class="sxs-lookup"><span data-stu-id="7c945-176">You see two folders: **inputdata** and **script**.</span></span>
7. <span data-ttu-id="7c945-177">Otevřete složku hello a zkontrolujte hello soubory ve složkách hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-177">Open hello folder and check hello files in hello folders.</span></span> <span data-ttu-id="7c945-178">Hello inputdata soubor input.log hello se vstupní data a složky hello skript obsahuje soubor skriptu HiveQL hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-178">hello inputdata contains hello input.log file with input data and hello script folder contains hello HiveQL script file.</span></span>

## <a name="create-a-data-factory-using-resource-manager-template"></a><span data-ttu-id="7c945-179">Vytvořte objekt pro vytváření dat pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="7c945-179">Create a data factory using Resource Manager template</span></span>
<span data-ttu-id="7c945-180">Účet úložiště hello, hello vstupní data a hello skript HiveQL připravený jsou připravené toocreate služby Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="7c945-180">With hello storage account, hello input data, and hello HiveQL script prepared, you are ready toocreate an Azure data factory.</span></span> <span data-ttu-id="7c945-181">Existuje několik metod pro vytvoření služby data factory.</span><span class="sxs-lookup"><span data-stu-id="7c945-181">There are several methods for creating data factory.</span></span> <span data-ttu-id="7c945-182">V tomto kurzu vytvoříte objekt pro vytváření dat nasazením šablonu Azure Resource Manager pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7c945-182">In this tutorial, you create a data factory by deploying an Azure Resource Manager template using hello Azure portal.</span></span> <span data-ttu-id="7c945-183">Můžete také nasadit šablony Resource Manageru pomocí [rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) a [prostředí Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="7c945-183">You can also deploy a Resource Manager template by using [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) and [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span> <span data-ttu-id="7c945-184">Ostatními metodami vytvoření objektu pro vytváření dat najdete v části [kurz: sestavení prvního objektu pro vytváření dat](../data-factory/data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="7c945-184">For other data factory creation methods, see [Tutorial: Build your first data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span></span>

1. <span data-ttu-id="7c945-185">Klikněte na tlačítko hello toosign bitové kopie v tooAzure a otevřete hello šablony Resource Manageru v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7c945-185">Click hello following image toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span> <span data-ttu-id="7c945-186">Šablona Hello je umístěna ve https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span><span class="sxs-lookup"><span data-stu-id="7c945-186">hello template is located at https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span></span> <span data-ttu-id="7c945-187">V tématu hello [entit služby Data Factory v šabloně hello](#data-factory-entities-in-the-template) části Podrobné informace o entitách definovaná v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-187">See hello [Data Factory entities in hello template](#data-factory-entities-in-the-template) section for detailed information about entities defined in hello template.</span></span> 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="7c945-188">Vyberte **použít existující** možnost pro hello **skupiny prostředků** nastavení a vyberte hello název skupiny prostředků hello jste vytvořili v předchozím kroku hello (pomocí skriptu prostředí PowerShell).</span><span class="sxs-lookup"><span data-stu-id="7c945-188">Select **Use existing** option for hello **Resource group** setting, and select hello name of hello resource group you created in hello previous step (using PowerShell script).</span></span>
3. <span data-ttu-id="7c945-189">Zadejte název objektu pro vytváření dat hello (**název objektu pro vytváření dat**).</span><span class="sxs-lookup"><span data-stu-id="7c945-189">Enter a name for hello data factory (**Data Factory Name**).</span></span> <span data-ttu-id="7c945-190">Tento název musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="7c945-190">This name must be globally unique.</span></span>
4. <span data-ttu-id="7c945-191">Zadejte hello **název účtu úložiště** a **klíč účtu úložiště** jste si poznamenali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-191">Enter hello **storage account name** and **storage account key** you wrote down in hello previous step.</span></span>
5. <span data-ttu-id="7c945-192">Vyberte **souhlasím toohello podmínky a ujednání** stanovené výše po přečtení **podmínky a ujednání**.</span><span class="sxs-lookup"><span data-stu-id="7c945-192">Select **I agree toohello terms and conditions** stated above after reading through **terms and conditions**.</span></span>
6. <span data-ttu-id="7c945-193">Vyberte **Pin toodashboard** možnost.</span><span class="sxs-lookup"><span data-stu-id="7c945-193">Select **Pin toodashboard** option.</span></span>
6. <span data-ttu-id="7c945-194">Klikněte na tlačítko **nákupu nebo vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7c945-194">Click **Purchase/Create**.</span></span> <span data-ttu-id="7c945-195">Zobrazí dlaždice na řídicí panel názvem hello **nasazení šablony**.</span><span class="sxs-lookup"><span data-stu-id="7c945-195">You see a tile on hello Dashboard called **Deploying Template deployment**.</span></span> <span data-ttu-id="7c945-196">Počkejte, dokud hello **skupiny prostředků** otevře se okno skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c945-196">Wait until hello **Resource group** blade for your resource group opens.</span></span> <span data-ttu-id="7c945-197">Můžete také kliknout na hello dlaždice s názvem jako vaše skupina název tooopen hello prostředků okně skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c945-197">You can also click hello tile titled as your resource group name tooopen hello resource group blade.</span></span>
6. <span data-ttu-id="7c945-198">Pokud okno skupiny prostředků hello není otevřený, klikněte na skupinu prostředků hello dlaždice tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-198">Click hello tile tooopen hello resource group if hello resource group blade is not already open.</span></span> <span data-ttu-id="7c945-199">Nyní se zobrazí jeden prostředek další objekt pro vytváření dat uvedené dále toohello úložiště účet prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c945-199">Now you shall see one more data factory resource listed in addition toohello storage account resource.</span></span>
7. <span data-ttu-id="7c945-200">Klikněte na tlačítko hello název objektu pro vytváření dat (hodnota zadaná pro hello **název objektu pro vytváření dat** parametr).</span><span class="sxs-lookup"><span data-stu-id="7c945-200">Click hello name of your data factory (value you specified for hello **Data Factory Name** parameter).</span></span>
8. <span data-ttu-id="7c945-201">V okně hello objekt pro vytváření dat, klikněte na tlačítko hello **Diagram** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7c945-201">In hello Data Factory blade, click hello **Diagram** tile.</span></span> <span data-ttu-id="7c945-202">Hello diagram znázorňuje jednu aktivitu s vstupní datové sady a výstupní datové:</span><span class="sxs-lookup"><span data-stu-id="7c945-202">hello diagram shows one activity with an input dataset, and an output dataset:</span></span>

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu diagram](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    <span data-ttu-id="7c945-204">názvy Hello jsou definovány v šabloně Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-204">hello names are defined in hello Resource Manager template.</span></span>
9. <span data-ttu-id="7c945-205">Klikněte dvakrát na **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="7c945-205">Double-click **AzureBlobOutput**.</span></span>
10. <span data-ttu-id="7c945-206">Na hello **poslední aktualizovat řezy**, zobrazí se jeden řez.</span><span class="sxs-lookup"><span data-stu-id="7c945-206">On hello **Recent updated slices**, you shall see one slice.</span></span> <span data-ttu-id="7c945-207">Pokud je stav hello **v průběhu**, počkejte, dokud se mění příliš**připraven**.</span><span class="sxs-lookup"><span data-stu-id="7c945-207">If hello status is **In progress**, wait until it is changed too**Ready**.</span></span> <span data-ttu-id="7c945-208">Obvykle trvá přibližně **20 minut** toocreate clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7c945-208">It usually takes about **20 minutes** toocreate an HDInsight cluster.</span></span>

### <a name="check-hello-data-factory-output"></a><span data-ttu-id="7c945-209">Zkontrolujte výstup objektu pro vytváření dat hello</span><span class="sxs-lookup"><span data-stu-id="7c945-209">Check hello data factory output</span></span>

1. <span data-ttu-id="7c945-210">Použití hello stejný postup v hello poslední relace toocheck hello kontejnery hello kontejneru adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="7c945-210">Use hello same procedure in hello last session toocheck hello containers of hello adfgetstarted container.</span></span> <span data-ttu-id="7c945-211">Existují dva nové kontejnery kromě příliš**adfgetsarted**:</span><span class="sxs-lookup"><span data-stu-id="7c945-211">There are two new containers in addition too**adfgetsarted**:</span></span>

   * <span data-ttu-id="7c945-212">Kontejner s názvem, který se následující hello: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="7c945-212">A container with name that follows hello pattern: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span></span> <span data-ttu-id="7c945-213">Tento kontejner je hello výchozí kontejner pro hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="7c945-213">This container is hello default container for hello HDInsight cluster.</span></span>
   * <span data-ttu-id="7c945-214">adfjobs: Tento kontejner je hello kontejner pro ADF hello v protokolech úloh.</span><span class="sxs-lookup"><span data-stu-id="7c945-214">adfjobs: This container is hello container for hello ADF job logs.</span></span>

     <span data-ttu-id="7c945-215">Výstup objektu pro vytváření dat Hello je uložen v **afgetstarted** jak jste nakonfigurovali v hello šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="7c945-215">hello data factory output is stored in **afgetstarted** as you configured in hello Resource Manager template.</span></span>
2. <span data-ttu-id="7c945-216">Klikněte na tlačítko **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="7c945-216">Click **adfgetstarted**.</span></span>
3. <span data-ttu-id="7c945-217">Klikněte dvakrát na **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="7c945-217">Double-click **partitioneddata**.</span></span> <span data-ttu-id="7c945-218">Zobrazí **rok = 2014** složky protože ze všech webových protokolů hello v roce 2014.</span><span class="sxs-lookup"><span data-stu-id="7c945-218">You see a **year=2014** folder because all hello web logs are dated in year 2014.</span></span>

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu výstup](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    <span data-ttu-id="7c945-220">Pokud přejdete k podrobnostem hello seznamu, zobrazí se tří složek pro leden, února a března.</span><span class="sxs-lookup"><span data-stu-id="7c945-220">If you drill down hello list, you shall see three folders for January, February, and March.</span></span> <span data-ttu-id="7c945-221">A je do protokolu pro každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="7c945-221">And there is a log for each month.</span></span>

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu výstup](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="7c945-223">Entity objektu pro vytváření dat v šabloně hello</span><span class="sxs-lookup"><span data-stu-id="7c945-223">Data Factory entities in hello template</span></span>
<span data-ttu-id="7c945-224">Zde je, jak vypadá hello nejvyšší úrovně šablony Resource Manageru pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="7c945-224">Here is how hello top-level Resource Manager template for a data factory looks like:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a><span data-ttu-id="7c945-225">Definování datové továrny</span><span class="sxs-lookup"><span data-stu-id="7c945-225">Define data factory</span></span>
<span data-ttu-id="7c945-226">Objekt pro vytváření dat v šabloně Resource Manager hello definovat, jak je uvedeno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="7c945-226">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
<span data-ttu-id="7c945-227">Hello dataFactoryName je hello název objektu pro vytváření dat hello, které určíte při nasazování šablony hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-227">hello dataFactoryName is hello name of hello data factory you specify when you deploy hello template.</span></span> <span data-ttu-id="7c945-228">Objekt pro vytváření dat se aktuálně podporuje jenom v oblasti Východ USA, Severní Evropa a západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-228">Data factory is currently only supported in hello East US, West US, and North Europe regions.</span></span>

### <a name="defining-entities-within-hello-data-factory"></a><span data-ttu-id="7c945-229">Definování entity v rámci objektu pro vytváření dat hello</span><span class="sxs-lookup"><span data-stu-id="7c945-229">Defining entities within hello data factory</span></span>
<span data-ttu-id="7c945-230">Hello následující entity služby Data Factory jsou definovány v šabloně hello JSON:</span><span class="sxs-lookup"><span data-stu-id="7c945-230">hello following Data Factory entities are defined in hello JSON template:</span></span>

* [<span data-ttu-id="7c945-231">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7c945-231">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="7c945-232">Propojená služba HDInsightu na vyžádání</span><span class="sxs-lookup"><span data-stu-id="7c945-232">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="7c945-233">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="7c945-233">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="7c945-234">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="7c945-234">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="7c945-235">Data Pipeline s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="7c945-235">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="7c945-236">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7c945-236">Azure Storage linked service</span></span>
<span data-ttu-id="7c945-237">Hello Azure Storage propojená služba propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="7c945-237">hello Azure Storage linked service links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="7c945-238">V tomto kurzu hello stejný účet úložiště se používá jako hello výchozí účet úložiště HDInsight, úložiště vstupní data a výstupní datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="7c945-238">In this tutorial, hello same storage account is used as hello default HDInsight storage account, input data storage, and output data storage.</span></span> <span data-ttu-id="7c945-239">Proto můžete definovat jen jeden Azure Storage, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7c945-239">Therefore, you define only one Azure Storage linked service.</span></span> <span data-ttu-id="7c945-240">V definici hello propojené služby je třeba zadat název hello a klíč účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7c945-240">In hello linked service definition, you specify hello name and key of your Azure storage account.</span></span> <span data-ttu-id="7c945-241">V tématu [propojená služba Azure Storage](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7c945-241">See [Azure Storage linked service](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span>

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
<span data-ttu-id="7c945-242">Hello **connectionString** používá hello storageAccountName a storageAccountKey parametry.</span><span class="sxs-lookup"><span data-stu-id="7c945-242">hello **connectionString** uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="7c945-243">Zadejte hodnoty pro tyto parametry při nasazování šablony hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-243">You specify values for these parameters while deploying hello template.</span></span>  

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="7c945-244">Propojená služba HDInsightu na vyžádání</span><span class="sxs-lookup"><span data-stu-id="7c945-244">HDInsight on-demand linked service</span></span>
<span data-ttu-id="7c945-245">V hello HDInsight na vyžádání propojené definice služby, zadejte hodnoty pro parametry konfigurace, které jsou používány toocreate služby Data Factory hello clusteru HDInsight Hadoop za běhu.</span><span class="sxs-lookup"><span data-stu-id="7c945-245">In hello on-demand HDInsight linked service definition, you specify values for configuration parameters that are used by hello Data Factory service toocreate a HDInsight Hadoop cluster at runtime.</span></span> <span data-ttu-id="7c945-246">V tématu [výpočetní propojené služby](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) článek podrobnosti o toodefine vlastnosti používané JSON propojené služby HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="7c945-246">See [Compute linked services](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used toodefine an HDInsight on-demand linked service.</span></span>  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
<span data-ttu-id="7c945-247">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="7c945-247">Note hello following points:</span></span>

* <span data-ttu-id="7c945-248">Hello Data Factory vytvoří **systémem Linux** HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="7c945-248">hello Data Factory creates a **Linux-based** HDInsight cluster for you.</span></span>
* <span data-ttu-id="7c945-249">Hello clusteru HDInsight Hadoop je vytvořen v hello stejné oblasti jako účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-249">hello HDInsight Hadoop cluster is created in hello same region as hello storage account.</span></span>
* <span data-ttu-id="7c945-250">Všimněte si hello *timeToLive* nastavení.</span><span class="sxs-lookup"><span data-stu-id="7c945-250">Notice hello *timeToLive* setting.</span></span> <span data-ttu-id="7c945-251">objekt pro vytváření dat Hello automaticky odstraní hello clusteru po hello clusteru se nečinnosti, po dobu 30 minut.</span><span class="sxs-lookup"><span data-stu-id="7c945-251">hello data factory deletes hello cluster automatically after hello cluster is being idle for 30 minutes.</span></span>
* <span data-ttu-id="7c945-252">Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="7c945-252">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="7c945-253">HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-253">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="7c945-254">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="7c945-254">This behavior is by design.</span></span> <span data-ttu-id="7c945-255">S na vyžádání propojené služby HDInsight, HDInsight cluster vytvoří pokaždé, když řez musí toobe zpracování, pokud neexistuje aktivní cluster (**timeToLive**) a po dokončení zpracování hello, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="7c945-255">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>

<span data-ttu-id="7c945-256">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="7c945-256">See [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c945-257">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="7c945-257">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="7c945-258">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="7c945-258">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="7c945-259">vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času".</span><span class="sxs-lookup"><span data-stu-id="7c945-259">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="7c945-260">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="7c945-260">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="7c945-261">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="7c945-261">Azure blob input dataset</span></span>
<span data-ttu-id="7c945-262">V definici hello vstupní datové sady zadejte názvy hello kontejner objektů blob, složku a soubor, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-262">In hello input dataset definition, you specify hello names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="7c945-263">V tématu [vlastnosti datové sady objektu Blob Azure](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="7c945-263">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}

```

<span data-ttu-id="7c945-264">Všimněte si následujících konkrétní nastavení v definici JSON hello hello:</span><span class="sxs-lookup"><span data-stu-id="7c945-264">Notice hello following specific settings in hello JSON definition:</span></span>

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="7c945-265">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="7c945-265">Azure Blob output dataset</span></span>
<span data-ttu-id="7c945-266">V definici datové sady výstup hello zadejte názvy hello kontejner objektů blob a složky, která obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="7c945-266">In hello output dataset definition, you specify hello names of blob container and folder that holds hello output data.</span></span> <span data-ttu-id="7c945-267">V tématu [vlastnosti datové sady objektu Blob Azure](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="7c945-267">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

<span data-ttu-id="7c945-268">Hello folderPath určuje hello cesta toohello složky, která obsahuje výstupní data hello:</span><span class="sxs-lookup"><span data-stu-id="7c945-268">hello folderPath specifies hello path toohello folder that holds hello output data:</span></span>

```json
"folderPath": "adfgetstarted/partitioneddata",
```

<span data-ttu-id="7c945-269">Hello [datovou sadu dostupnosti](../data-factory/data-factory-create-datasets.md#dataset-availability) nastavení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="7c945-269">hello [dataset availability](../data-factory/data-factory-create-datasets.md#dataset-availability) setting is as follows:</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

<span data-ttu-id="7c945-270">V Azure Data Factory, výstupní datovou sadu dostupnosti jednotky hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="7c945-270">In Azure Data Factory, output dataset availability drives hello pipeline.</span></span> <span data-ttu-id="7c945-271">V tomto příkladu hello řez vytváří jednou měsíčně hello poslední den v měsíci (EndOfInterval).</span><span class="sxs-lookup"><span data-stu-id="7c945-271">In this example, hello slice is produced monthly on hello last day of month (EndOfInterval).</span></span> <span data-ttu-id="7c945-272">Další informace najdete v tématu [Data Factory plánování a provádění](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="7c945-272">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

#### <a name="data-pipeline"></a><span data-ttu-id="7c945-273">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="7c945-273">Data pipeline</span></span>
<span data-ttu-id="7c945-274">Můžete definovat kanál, který transformuje data pomocí skriptu Hive v clusteru Azure HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="7c945-274">You define a pipeline that transforms data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="7c945-275">V tématu [JSON kanálu](../data-factory/data-factory-create-pipelines.md#pipeline-json) popisy toodefine prvky používané JSON kanálu v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="7c945-275">See [Pipeline JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span>

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="7c945-276">Hello kanál obsahuje jednu aktivitu, aktivita HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="7c945-276">hello pipeline contains one activity, HDInsightHive activity.</span></span> <span data-ttu-id="7c945-277">Jako počáteční a koncová data jsou v leden 2016, data pro pouze jeden měsíc () zpracování řezu se.</span><span class="sxs-lookup"><span data-stu-id="7c945-277">As both start and end dates are in January 2016, data for only one month (a slice) is processed.</span></span> <span data-ttu-id="7c945-278">Obě *spustit* a *end* hello aktivity mají na datum v minulosti, takže hello objekt pro vytváření dat zpracovává data pro měsíc hello okamžitě.</span><span class="sxs-lookup"><span data-stu-id="7c945-278">Both *start* and *end* of hello activity have a past date, so hello Data Factory processes data for hello month immediately.</span></span> <span data-ttu-id="7c945-279">Pokud koncový hello je budoucí datum, hello data factory vytvoří jiné řez, pokud nastane čas hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-279">If hello end is a future date, hello data factory creates another slice when hello time comes.</span></span> <span data-ttu-id="7c945-280">Další informace najdete v tématu [Data Factory plánování a provádění](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="7c945-280">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

## <a name="clean-up-hello-tutorial"></a><span data-ttu-id="7c945-281">Vyčistěte kurz hello</span><span class="sxs-lookup"><span data-stu-id="7c945-281">Clean up hello tutorial</span></span>

### <a name="delete-hello-blob-containers-created-by-on-demand-hdinsight-cluster"></a><span data-ttu-id="7c945-282">Odstranění kontejnerů objektů blob hello vytvořené clusteru HDInsight na vyžádání</span><span class="sxs-lookup"><span data-stu-id="7c945-282">Delete hello blob containers created by on-demand HDInsight cluster</span></span>
<span data-ttu-id="7c945-283">S na vyžádání propojené služby HDInsight HDInsight cluster vytvoří pokaždé, když řez musí toobe zpracování, pokud neexistuje aktivní cluster (timeToLive); a hello clusteru se odstraní po dokončení zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-283">With on-demand HDInsight linked service, an HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (timeToLive); and hello cluster is deleted when hello processing is done.</span></span> <span data-ttu-id="7c945-284">Pro každý cluster Azure Data Factory vytvoří kontejner objektů blob v hello úložiště objektů blob v Azure jako hello výchozí stroage účet pro hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="7c945-284">For each cluster, Azure Data Factory creates a blob container in hello Azure blob storage used as hello default stroage account for hello cluster.</span></span> <span data-ttu-id="7c945-285">To i v případě odstranění clusteru HDInsight, nebudou odstraněny hello výchozího kontejneru blob storage a hello přidruženého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7c945-285">Even though HDInsight cluster is deleted, hello default blob storage container and hello associated storage account are not deleted.</span></span> <span data-ttu-id="7c945-286">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="7c945-286">This behavior is by design.</span></span> <span data-ttu-id="7c945-287">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="7c945-287">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="7c945-288">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="7c945-288">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="7c945-289">vzor podle Hello názvy těchto kontejnerů: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="7c945-289">hello names of these containers follow a pattern: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span></span>

<span data-ttu-id="7c945-290">Odstranit hello **adfjobs** a **adfyourdatafactoryname-linkedservicename razítko_data_a_času** složky.</span><span class="sxs-lookup"><span data-stu-id="7c945-290">Delete hello **adfjobs** and **adfyourdatafactoryname-linkedservicename-datetimestamp** folders.</span></span> <span data-ttu-id="7c945-291">Hello adfjobs kontejner obsahuje protokoly úlohy Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7c945-291">hello adfjobs container contains job logs from Azure Data Factory.</span></span>

### <a name="delete-hello-resource-group"></a><span data-ttu-id="7c945-292">Odstraňte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="7c945-292">Delete hello resource group</span></span>
<span data-ttu-id="7c945-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) je použité toodeploy, správu a sledování řešení jako se skupinou.</span><span class="sxs-lookup"><span data-stu-id="7c945-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) is used toodeploy, manage, and monitor your solution as a group.</span></span>  <span data-ttu-id="7c945-294">Odstranění skupiny prostředků se odstraní všechny součásti hello uvnitř skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-294">Deleting a resource group deletes all hello components inside hello group.</span></span>  

1. <span data-ttu-id="7c945-295">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7c945-295">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7c945-296">Klikněte na tlačítko **skupiny prostředků** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-296">Click **Resource groups** on hello left pane.</span></span>
3. <span data-ttu-id="7c945-297">Klikněte na název skupiny prostředků hello, kterou jste vytvořili ve vašem skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7c945-297">Click hello resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="7c945-298">Pomocí filtru hello, pokud máte příliš mnoho skupin prostředků, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="7c945-298">Use hello filter if you have too many resource groups listed.</span></span> <span data-ttu-id="7c945-299">Hello skupiny prostředků se otevře v novém okně.</span><span class="sxs-lookup"><span data-stu-id="7c945-299">It opens hello resource group in a new blade.</span></span>
4. <span data-ttu-id="7c945-300">Na hello **prostředky** dlaždice, musí mít hello výchozí účet úložiště a hello data factory uvedené Pokud sdílíte s další projekty skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-300">On hello **Resources** tile, you shall have hello default storage account and hello data factory listed unless you share hello resource group with other projects.</span></span>
5. <span data-ttu-id="7c945-301">Klikněte na tlačítko **odstranit** na hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-301">Click **Delete** on hello top of hello blade.</span></span> <span data-ttu-id="7c945-302">Díky tomu odstraní účet úložiště hello a hello data uložená v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-302">Doing so deletes hello storage account and hello data stored in hello storage account.</span></span>
6. <span data-ttu-id="7c945-303">Zadejte odstranění tooconfirm hello prostředků skupiny název a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="7c945-303">Enter hello resource group name tooconfirm deletion, and then click **Delete**.</span></span>

<span data-ttu-id="7c945-304">V případě, že při odstranění skupiny prostředků hello nechcete, aby účet úložiště hello toodelete, vezměte v úvahu hello následující architektura oddělením hello podniková data z hello výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7c945-304">In case you don't want toodelete hello storage account when you delete hello resource group, consider hello following architecture by separating hello business data from hello default storage account.</span></span> <span data-ttu-id="7c945-305">V takovém případě máte jednu skupinu prostředků pro účet úložiště hello s hello obchodních dat a hello jiné skupině prostředků pro hello výchozí účet úložiště pro HDInsight propojené služby a hello služby data factory.</span><span class="sxs-lookup"><span data-stu-id="7c945-305">In this case, you have one resource group for hello storage account with hello business data, and hello other resource group for hello default storage account for HDInsight linked service and hello data factory.</span></span> <span data-ttu-id="7c945-306">Při odstranění skupiny prostředků druhý hello neovlivní účet úložiště dat hello firmy.</span><span class="sxs-lookup"><span data-stu-id="7c945-306">When you delete hello second resource group, it does not impact hello business data storage account.</span></span> <span data-ttu-id="7c945-307">toodo tak:</span><span class="sxs-lookup"><span data-stu-id="7c945-307">toodo so:</span></span>

* <span data-ttu-id="7c945-308">Přidejte hello následující skupiny nejvyšší úrovně prostředků toohello společně s hello Microsoft.DataFactory/datafactories prostředků ve vaší šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7c945-308">Add hello following toohello top-level resource group along with hello Microsoft.DataFactory/datafactories resource in your Resource Manager template.</span></span> <span data-ttu-id="7c945-309">Vytvoří účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="7c945-309">It creates a storage account:</span></span>

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* <span data-ttu-id="7c945-310">Přidejte nové propojené služby bodu toohello nový účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="7c945-310">Add a new linked service point toohello new storage account:</span></span>

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* <span data-ttu-id="7c945-311">Nakonfigurujte další dependsOn a additionalLinkedServiceNames hello HDInsight ondemand propojené služby:</span><span class="sxs-lookup"><span data-stu-id="7c945-311">Configure hello HDInsight ondemand linked service with an additional dependsOn and an additionalLinkedServiceNames:</span></span>

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a><span data-ttu-id="7c945-312">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7c945-312">Next steps</span></span>
<span data-ttu-id="7c945-313">V tomto článku jste se naučili jak toouse Azure Data Factory toocreate na vyžádání HDInsight clusteru tooprocess úloh Hive.</span><span class="sxs-lookup"><span data-stu-id="7c945-313">In this article, you have learned how toouse Azure Data Factory toocreate on-demand HDInsight cluster tooprocess Hive jobs.</span></span> <span data-ttu-id="7c945-314">Další tooread:</span><span class="sxs-lookup"><span data-stu-id="7c945-314">tooread more:</span></span>

* [<span data-ttu-id="7c945-315">Kurz Hadoopu: začněte používat systémem Linux Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7c945-315">Hadoop tutorial: Get started using Linux-based Hadoop in HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="7c945-316">Vytvořit clustery se systémem Linux Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7c945-316">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="7c945-317">Dokumentace prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="7c945-317">HDInsight documentation</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="7c945-318">Dokumentace k objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="7c945-318">Data factory documentation</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a><span data-ttu-id="7c945-319">Příloha</span><span class="sxs-lookup"><span data-stu-id="7c945-319">Appendix</span></span>

### <a name="azure-cli-script"></a><span data-ttu-id="7c945-320">Azure CLI skriptu</span><span class="sxs-lookup"><span data-stu-id="7c945-320">Azure CLI script</span></span>
<span data-ttu-id="7c945-321">Místo použití prostředí Azure PowerShell toodo hello kurzu můžete použít rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="7c945-321">You can use Azure CLI instead of using Azure PowerShell toodo hello tutorial.</span></span> <span data-ttu-id="7c945-322">toouse rozhraní příkazového řádku Azure, nejprve nainstalujte rozhraní příkazového řádku Azure podle pokynů hello:</span><span class="sxs-lookup"><span data-stu-id="7c945-322">toouse Azure CLI, first install Azure CLI as per hello following instructions:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-tooprepare-hello-storage-and-copy-hello-files"></a><span data-ttu-id="7c945-323">Používat rozhraní příkazového řádku Azure tooprepare hello úložiště a kopírovat soubory hello</span><span class="sxs-lookup"><span data-stu-id="7c945-323">Use Azure CLI tooprepare hello storage and copy hello files</span></span>

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

<span data-ttu-id="7c945-324">název kontejneru Hello je *adfgetstarted*.</span><span class="sxs-lookup"><span data-stu-id="7c945-324">hello container name is *adfgetstarted*.</span></span> <span data-ttu-id="7c945-325">Protože se jedná, uchovávejte ho.</span><span class="sxs-lookup"><span data-stu-id="7c945-325">Keep it as it is.</span></span> <span data-ttu-id="7c945-326">V opačném případě je nutné šablony Resource Manageru tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="7c945-326">Otherwise you need tooupdate hello Resource Manager template.</span></span> <span data-ttu-id="7c945-327">Pokud potřebujete pomoc s Tento skript rozhraní příkazového řádku, přečtěte si [hello pomocí rozhraní příkazového řádku Azure s Azure Storage](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7c945-327">If you need help with this CLI script, see [Using hello Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>
