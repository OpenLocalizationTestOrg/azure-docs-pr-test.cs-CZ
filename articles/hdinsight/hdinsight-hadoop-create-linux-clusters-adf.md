---
title: "Vytváření clusterů systému Hadoop na vyžádání pomocí služby Data Factory - Azure HDInsight | Microsoft Docs"
description: "Naučte se vytvářet na vyžádání clusterů systému Hadoop v HDInsight pomocí Azure Data Factory."
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
ms.openlocfilehash: e68f1d72965d9516e0552c84d03d234c21739390
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a><span data-ttu-id="87014-103">Vytvářet na vyžádání clusterů systému Hadoop v HDInsight pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="87014-103">Create on-demand Hadoop clusters in HDInsight using Azure Data Factory</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="87014-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) je služba integrace cloudových dat, která orchestruje a automatizuje přesouvání a transformaci dat.</span><span class="sxs-lookup"><span data-stu-id="87014-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) is a cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="87014-105">Může vytvořit HDInsight Hadoop clusteru v běhu ke zpracování řez vstupních dat a po dokončení zpracování odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="87014-105">It can create a HDInsight Hadoop cluster just-in-time to process an input data slice and delete the cluster when the processing is complete.</span></span> <span data-ttu-id="87014-106">Některé z výhod používání cluster systému HDInsight Hadoop na vyžádání jsou:</span><span class="sxs-lookup"><span data-stu-id="87014-106">Some of the benefits of using an on-demand HDInsight Hadoop cluster are:</span></span>

- <span data-ttu-id="87014-107">Můžete pouze platím čas úlohy běží na clusteru HDInsight Hadoop (plus krátké doby nečinnosti konfigurovat).</span><span class="sxs-lookup"><span data-stu-id="87014-107">You only pay for the time job is running on the HDInsight Hadoop cluster (plus a brief configurable idle time).</span></span> <span data-ttu-id="87014-108">Fakturace pro clustery služby HDInsight se fakturují za minutu, zda jsou jejich používání, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="87014-108">The billing for HDInsight clusters is pro-rated per minute, whether you are using them or not.</span></span> <span data-ttu-id="87014-109">Při použití na vyžádání propojené služby HDInsight v objektu pro vytváření dat, jsou vytvářet clustery na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="87014-109">When you use an on-demand HDInsight linked service in Data Factory, the clusters are created on-demand.</span></span> <span data-ttu-id="87014-110">A clustery jsou automaticky odstraněny při dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="87014-110">And the clusters are deleted automatically when the jobs are completed.</span></span> <span data-ttu-id="87014-111">Proto platíte jenom pro úlohu s krátké době nečinnosti (nastavení time to live).</span><span class="sxs-lookup"><span data-stu-id="87014-111">Therefore, you only pay for the job running time and the brief idle time (time-to-live setting).</span></span>
- <span data-ttu-id="87014-112">Můžete vytvořit pracovní postup pomocí objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="87014-112">You can create a workflow using a Data Factory pipeline.</span></span> <span data-ttu-id="87014-113">Například můžete mít kanál kopírování dat z místního serveru SQL do Azure blob storage, zpracování dat při spuštění skriptu Hive a Pig skript na cluster systému HDInsight Hadoop na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="87014-113">For example, you can have the pipeline to copy data from an on-premises SQL Server to an Azure blob storage, process the data by running a Hive script and a Pig script on an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="87014-114">Zkopírujte výsledná data do Azure SQL Data Warehouse pro BI aplikace využívat.</span><span class="sxs-lookup"><span data-stu-id="87014-114">Then, copy the result data to an Azure SQL Data Warehouse for BI applications to consume.</span></span>
- <span data-ttu-id="87014-115">Můžete naplánovat pracovní postup spustit pravidelně (HODINOVĚ, denně, týdně, měsíčně, atd.).</span><span class="sxs-lookup"><span data-stu-id="87014-115">You can schedule the workflow to run periodically (hourly, daily, weekly, monthly, etc.).</span></span>

<span data-ttu-id="87014-116">V Azure Data Factory objekt pro vytváření dat může mít jeden nebo více datových kanálů.</span><span class="sxs-lookup"><span data-stu-id="87014-116">In Azure Data Factory, a data factory can have one or more data pipelines.</span></span> <span data-ttu-id="87014-117">Datový kanál obsahuje jeden nebo více aktivit.</span><span class="sxs-lookup"><span data-stu-id="87014-117">A data pipeline has one or more activities.</span></span> <span data-ttu-id="87014-118">Existují dva typy aktivit: [aktivity přesunu dat](../data-factory/data-factory-data-movement-activities.md) a [aktivit transformace dat](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="87014-118">There are two types of activities: [Data Movement Activities](../data-factory/data-factory-data-movement-activities.md) and [Data Transformation Activities](../data-factory/data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="87014-119">Pro přesun dat z jiného úložiště dat zdrojového do cílového úložiště dat použijete aktivity přesunu dat (v současné době pouze aktivita kopírování).</span><span class="sxs-lookup"><span data-stu-id="87014-119">You use data movement activities (currently, only Copy Activity) to move data from a source data store to a destination data store.</span></span> <span data-ttu-id="87014-120">Aktivity transformace dat použijete transformace nebo zpracovat data.</span><span class="sxs-lookup"><span data-stu-id="87014-120">You use data transformation activities to transform/process data.</span></span> <span data-ttu-id="87014-121">Aktivita HDInsight Hive je jedním z aktivit transformace podporovaných službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="87014-121">HDInsight Hive Activity is one of the transformation activities supported by Data Factory.</span></span> <span data-ttu-id="87014-122">V tomto kurzu použijete aktivitu transformace Hive.</span><span class="sxs-lookup"><span data-stu-id="87014-122">You use the Hive transformation activity in this tutorial.</span></span>

<span data-ttu-id="87014-123">Můžete nakonfigurovat aktivitu hive používat vlastní cluster HDInsight Hadoop nebo cluster systému HDInsight Hadoop na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="87014-123">You can configure a hive activity to use your own HDInsight Hadoop cluster or an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="87014-124">V tomto kurzu aktivity Hive v kanálu objekt pro vytváření dat je nakonfigurován na používání clusteru HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="87014-124">In this tutorial, the Hive activity in the data factory pipeline is configured to use an on-demand HDInsight cluster.</span></span> <span data-ttu-id="87014-125">Proto při spuštění aktivity ke zpracování dat řezu, stane se toto:</span><span class="sxs-lookup"><span data-stu-id="87014-125">Therefore, when the activity runs to process a data slice, here is what happens:</span></span>

1. <span data-ttu-id="87014-126">Pro můžete v běhu při zpracování řezu se automaticky vytvoří cluster HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="87014-126">A HDInsight Hadoop cluster is automatically created for you just-in-time to process the slice.</span></span>  
2. <span data-ttu-id="87014-127">Spuštění skriptu HiveQL v clusteru zpracovává vstupní data.</span><span class="sxs-lookup"><span data-stu-id="87014-127">The input data is processed by running a HiveQL script on the cluster.</span></span>
3. <span data-ttu-id="87014-128">Po dokončení zpracování a cluster nakonfigurovaného množství času (nastavení timeToLive) nečinný odstranění clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="87014-128">The HDInsight Hadoop cluster is deleted after the processing is complete and the cluster is idle for the configured amount of time (timeToLive setting).</span></span> <span data-ttu-id="87014-129">Pokud další datový řez je dostupný pro zpracování s timeToLive dobu nečinnosti, stejného clusteru se používá ke zpracování řezu.</span><span class="sxs-lookup"><span data-stu-id="87014-129">If the next data slice is available for processing with in this timeToLive idle time, the same cluster is used to process the slice.</span></span>  

<span data-ttu-id="87014-130">V tomto kurzu skript HiveQL přidružený k aktivitě hive provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="87014-130">In this tutorial, the HiveQL script associated with the hive activity performs the following actions:</span></span>

1. <span data-ttu-id="87014-131">Vytvoří externí tabulku, která odkazuje na data protokolu nezpracovaná webové uložené v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="87014-131">Creates an external table that references the raw web log data stored in an Azure Blob storage.</span></span>
2. <span data-ttu-id="87014-132">Oddíly nezpracovaná data podle roku a měsíce.</span><span class="sxs-lookup"><span data-stu-id="87014-132">Partitions the raw data by year and month.</span></span>
3. <span data-ttu-id="87014-133">Ukládá oddílů data do Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="87014-133">Stores the partitioned data in the Azure blob storage.</span></span>

<span data-ttu-id="87014-134">V tomto kurzu skript HiveQL přidružený k aktivitě hive vytvoří externí tabulku, která odkazuje na nezpracovaná webové protokolu data uložená ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="87014-134">In this tutorial, the HiveQL script associated with the hive activity creates an external table that references the raw web log data stored in the Azure Blob Storage.</span></span> <span data-ttu-id="87014-135">Zde jsou řádky vzorku pro každý měsíc ve vstupním souboru.</span><span class="sxs-lookup"><span data-stu-id="87014-135">Here are the sample rows for each month in the input file.</span></span>

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

<span data-ttu-id="87014-136">Skript HiveQL oddíly nezpracovaná data podle roku a měsíce.</span><span class="sxs-lookup"><span data-stu-id="87014-136">The HiveQL script partitions the raw data by year and month.</span></span> <span data-ttu-id="87014-137">Vytvoří tři výstupní složky podle předchozích vstup.</span><span class="sxs-lookup"><span data-stu-id="87014-137">It creates three output folders based on the previous input.</span></span> <span data-ttu-id="87014-138">Daná složka obsahuje soubor s položky z každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="87014-138">Each folder contains a file with entries from each month.</span></span>

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

<span data-ttu-id="87014-139">Seznam aktivit transformace dat služby Data Factory kromě Hive aktivity, naleznete v části [transformovat a analyzovat pomocí Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="87014-139">For a list of Data Factory data transformation activities in addition to Hive activity, see [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="87014-140">V současné době můžete vytvořit pouze verze clusteru HDInsight 3.2 z Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="87014-140">Currently, you can only create HDInsight cluster version 3.2 from Azure Data Factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87014-141">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87014-141">Prerequisites</span></span>
<span data-ttu-id="87014-142">Než začnete plnit pokyny v tomto článku, musíte mít následující položky:</span><span class="sxs-lookup"><span data-stu-id="87014-142">Before you begin the instructions in this article, you must have the following items:</span></span>

* <span data-ttu-id="87014-143">[Předplatné Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="87014-143">[Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="87014-144">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="87014-144">Azure PowerShell.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a><span data-ttu-id="87014-145">Příprava účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="87014-145">Prepare storage account</span></span>
<span data-ttu-id="87014-146">V tomto scénáři můžete použít až tři účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="87014-146">You can use up to three storage accounts in this scenario:</span></span>

- <span data-ttu-id="87014-147">Výchozí účet úložiště pro HDInsight cluster</span><span class="sxs-lookup"><span data-stu-id="87014-147">default storage account for the HDInsight cluster</span></span>
- <span data-ttu-id="87014-148">účet úložiště pro vstupních dat</span><span class="sxs-lookup"><span data-stu-id="87014-148">storage account for the input data</span></span>
- <span data-ttu-id="87014-149">účet úložiště pro výstupní data</span><span class="sxs-lookup"><span data-stu-id="87014-149">storage account for the output data</span></span>

<span data-ttu-id="87014-150">Pro zjednodušení tento kurz, použijete jeden účet úložiště k obsluze tři účely.</span><span class="sxs-lookup"><span data-stu-id="87014-150">To simplify the tutorial, you use one storage account to serve the three purposes.</span></span> <span data-ttu-id="87014-151">Najít v této části prostředí Azure PowerShell ukázkový skript provede následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="87014-151">The Azure PowerShell sample script found in this section performs the following tasks:</span></span>

1. <span data-ttu-id="87014-152">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="87014-152">Log in to Azure.</span></span>
2. <span data-ttu-id="87014-153">Vytvořte skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="87014-153">Create an Azure resource group.</span></span>
3. <span data-ttu-id="87014-154">Vytvoření účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="87014-154">Create an Azure Storage account.</span></span>
4. <span data-ttu-id="87014-155">Vytvořte kontejner objektů Blob v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="87014-155">Create a Blob container in the storage account</span></span>
5. <span data-ttu-id="87014-156">Zkopírujte následující dva soubory do kontejneru objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="87014-156">Copy the following two files to the Blob container:</span></span>

   * <span data-ttu-id="87014-157">Vstupní datový soubor: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span><span class="sxs-lookup"><span data-stu-id="87014-157">Input data file: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span></span>
   * <span data-ttu-id="87014-158">Skript HiveQL: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span><span class="sxs-lookup"><span data-stu-id="87014-158">HiveQL script: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span></span>

     <span data-ttu-id="87014-159">Oba soubory jsou uloženy ve veřejném kontejneru Blob.</span><span class="sxs-lookup"><span data-stu-id="87014-159">Both files are stored in a public Blob container.</span></span>


<span data-ttu-id="87014-160">**Příprava úložiště a kopírovat soubory pomocí Azure PowerShell:**</span><span class="sxs-lookup"><span data-stu-id="87014-160">**To prepare the storage and copy the files using Azure PowerShell:**</span></span>
> [!IMPORTANT]
> <span data-ttu-id="87014-161">Zadejte názvy pro skupinu prostředků Azure a účet úložiště Azure, který bude vytvořen skriptem.</span><span class="sxs-lookup"><span data-stu-id="87014-161">Specify names for the Azure resource group and the Azure storage account that will be created by the script.</span></span>
> <span data-ttu-id="87014-162">Zapište **název skupiny prostředků**, **název účtu úložiště**, a **klíč účtu úložiště** výstupem skriptem.</span><span class="sxs-lookup"><span data-stu-id="87014-162">Write down **resource group name**, **storage account name**, and **storage account key** outputted by the script.</span></span> <span data-ttu-id="87014-163">Je nutné v další části.</span><span class="sxs-lookup"><span data-stu-id="87014-163">You need them in the next section.</span></span>

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
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

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

<span data-ttu-id="87014-164">Pokud potřebujete pomoc s skript prostředí PowerShell, přečtěte si [pomocí Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="87014-164">If you need help with the PowerShell script, see [Using the Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span> <span data-ttu-id="87014-165">Pokud chcete místo toho použijte rozhraní příkazového řádku Azure, najdete v článku [příloha](#appendix) části pro skript příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="87014-165">If you like to use Azure CLI instead, see the [Appendix](#appendix) section for the Azure CLI script.</span></span>

<span data-ttu-id="87014-166">**K prozkoumání účet úložiště a obsah**</span><span class="sxs-lookup"><span data-stu-id="87014-166">**To examine the storage account and the contents**</span></span>

1. <span data-ttu-id="87014-167">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="87014-167">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="87014-168">Klikněte na tlačítko **skupiny prostředků** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="87014-168">Click **Resource groups** on the left pane.</span></span>
3. <span data-ttu-id="87014-169">Dvakrát klikněte na název skupiny prostředků, kterou jste vytvořili ve vašem skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87014-169">Double-click the resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="87014-170">Pokud máte příliš mnoho skupin prostředků, které jsou uvedeny, použijte filtr.</span><span class="sxs-lookup"><span data-stu-id="87014-170">Use the filter if you have too many resource groups listed.</span></span>
4. <span data-ttu-id="87014-171">Na **prostředky** dlaždice, musí mít jeden prostředek uvedené Pokud sdílíte s jinými projekty skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="87014-171">On the **Resources** tile, you shall have one resource listed unless you share the resource group with other projects.</span></span> <span data-ttu-id="87014-172">Tento prostředek je účet úložiště s názvem, který jste zadali dříve.</span><span class="sxs-lookup"><span data-stu-id="87014-172">That resource is the storage account with the name you specified earlier.</span></span> <span data-ttu-id="87014-173">Klikněte na název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-173">Click the storage account name.</span></span>
5. <span data-ttu-id="87014-174">Klikněte **objekty BLOB** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="87014-174">Click the **Blobs** tiles.</span></span>
6. <span data-ttu-id="87014-175">Klikněte **adfgetstarted** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="87014-175">Click the **adfgetstarted** container.</span></span> <span data-ttu-id="87014-176">Zobrazí dvě složky: **inputdata** a **skriptu**.</span><span class="sxs-lookup"><span data-stu-id="87014-176">You see two folders: **inputdata** and **script**.</span></span>
7. <span data-ttu-id="87014-177">Otevřete složku a zkontrolujte soubory ve složkách.</span><span class="sxs-lookup"><span data-stu-id="87014-177">Open the folder and check the files in the folders.</span></span> <span data-ttu-id="87014-178">Inputdata soubor input.log s vstupní data a složky skript obsahuje soubor skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="87014-178">The inputdata contains the input.log file with input data and the script folder contains the HiveQL script file.</span></span>

## <a name="create-a-data-factory-using-resource-manager-template"></a><span data-ttu-id="87014-179">Vytvořte objekt pro vytváření dat pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="87014-179">Create a data factory using Resource Manager template</span></span>
<span data-ttu-id="87014-180">Účet úložiště, vstupní data a skript HiveQL připravený jste připraveni k vytvoření služby Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="87014-180">With the storage account, the input data, and the HiveQL script prepared, you are ready to create an Azure data factory.</span></span> <span data-ttu-id="87014-181">Existuje několik metod pro vytvoření služby data factory.</span><span class="sxs-lookup"><span data-stu-id="87014-181">There are several methods for creating data factory.</span></span> <span data-ttu-id="87014-182">V tomto kurzu vytvoříte objekt pro vytváření dat nasazením šablonu Azure Resource Manager pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87014-182">In this tutorial, you create a data factory by deploying an Azure Resource Manager template using the Azure portal.</span></span> <span data-ttu-id="87014-183">Můžete také nasadit šablony Resource Manageru pomocí [rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) a [prostředí Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="87014-183">You can also deploy a Resource Manager template by using [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) and [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span> <span data-ttu-id="87014-184">Ostatními metodami vytvoření objektu pro vytváření dat najdete v části [kurz: sestavení prvního objektu pro vytváření dat](../data-factory/data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="87014-184">For other data factory creation methods, see [Tutorial: Build your first data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span></span>

1. <span data-ttu-id="87014-185">Klikněte na následující obrázek pro přihlášení do Azure a otevřete šablonu Resource Manageru na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="87014-185">Click the following image to sign in to Azure and open the Resource Manager template in the Azure portal.</span></span> <span data-ttu-id="87014-186">Šablona se nachází v https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span><span class="sxs-lookup"><span data-stu-id="87014-186">The template is located at https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span></span> <span data-ttu-id="87014-187">Najdete v článku [entit služby Data Factory v šabloně](#data-factory-entities-in-the-template) části Podrobné informace o entit definované v šabloně.</span><span class="sxs-lookup"><span data-stu-id="87014-187">See the [Data Factory entities in the template](#data-factory-entities-in-the-template) section for detailed information about entities defined in the template.</span></span> 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. <span data-ttu-id="87014-188">Vyberte **použít existující** možnost **skupiny prostředků** nastavení a vyberte název skupiny prostředků, kterou jste vytvořili v předchozím kroku (pomocí skriptu prostředí PowerShell).</span><span class="sxs-lookup"><span data-stu-id="87014-188">Select **Use existing** option for the **Resource group** setting, and select the name of the resource group you created in the previous step (using PowerShell script).</span></span>
3. <span data-ttu-id="87014-189">Zadejte název objektu pro vytváření dat (**název objektu pro vytváření dat**).</span><span class="sxs-lookup"><span data-stu-id="87014-189">Enter a name for the data factory (**Data Factory Name**).</span></span> <span data-ttu-id="87014-190">Tento název musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="87014-190">This name must be globally unique.</span></span>
4. <span data-ttu-id="87014-191">Zadejte **název účtu úložiště** a **klíč účtu úložiště** jste si poznamenali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="87014-191">Enter the **storage account name** and **storage account key** you wrote down in the previous step.</span></span>
5. <span data-ttu-id="87014-192">Vyberte **souhlasím s podmínkami a ujednáními** stanovené výše po přečtení **podmínky a ujednání**.</span><span class="sxs-lookup"><span data-stu-id="87014-192">Select **I agree to the terms and conditions** stated above after reading through **terms and conditions**.</span></span>
6. <span data-ttu-id="87014-193">Vyberte **připnout na řídicí panel** možnost.</span><span class="sxs-lookup"><span data-stu-id="87014-193">Select **Pin to dashboard** option.</span></span>
6. <span data-ttu-id="87014-194">Klikněte na tlačítko **nákupu nebo vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="87014-194">Click **Purchase/Create**.</span></span> <span data-ttu-id="87014-195">Zobrazí na dlaždici na řídicím panelu názvem **nasazení šablony**.</span><span class="sxs-lookup"><span data-stu-id="87014-195">You see a tile on the Dashboard called **Deploying Template deployment**.</span></span> <span data-ttu-id="87014-196">Počkejte **skupiny prostředků** otevře se okno skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="87014-196">Wait until the **Resource group** blade for your resource group opens.</span></span> <span data-ttu-id="87014-197">Můžete také kliknutím na dlaždici s názvem jako název skupiny prostředků a otevřete okno skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="87014-197">You can also click the tile titled as your resource group name to open the resource group blade.</span></span>
6. <span data-ttu-id="87014-198">Kliknutím na dlaždici otevřít skupinu prostředků, je-li okně skupiny prostředků není otevřený.</span><span class="sxs-lookup"><span data-stu-id="87014-198">Click the tile to open the resource group if the resource group blade is not already open.</span></span> <span data-ttu-id="87014-199">Nyní se zobrazí jeden další prostředek objektu pro vytváření dat uvedené kromě prostředků účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-199">Now you shall see one more data factory resource listed in addition to the storage account resource.</span></span>
7. <span data-ttu-id="87014-200">Klikněte na název objektu pro vytváření dat (hodnota, kterou jste zadali pro **název objektu pro vytváření dat** parametr).</span><span class="sxs-lookup"><span data-stu-id="87014-200">Click the name of your data factory (value you specified for the **Data Factory Name** parameter).</span></span>
8. <span data-ttu-id="87014-201">V okně Data Factory klikněte **Diagram** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="87014-201">In the Data Factory blade, click the **Diagram** tile.</span></span> <span data-ttu-id="87014-202">Diagram znázorňuje jednu aktivitu s vstupní datové sady a výstupní datové:</span><span class="sxs-lookup"><span data-stu-id="87014-202">The diagram shows one activity with an input dataset, and an output dataset:</span></span>

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu diagram](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    <span data-ttu-id="87014-204">Názvy jsou definovány v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="87014-204">The names are defined in the Resource Manager template.</span></span>
9. <span data-ttu-id="87014-205">Klikněte dvakrát na **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="87014-205">Double-click **AzureBlobOutput**.</span></span>
10. <span data-ttu-id="87014-206">Na **poslední aktualizovat řezy**, se zobrazí jeden řez.</span><span class="sxs-lookup"><span data-stu-id="87014-206">On the **Recent updated slices**, you shall see one slice.</span></span> <span data-ttu-id="87014-207">Pokud je stav **v průběhu**, počkejte, dokud se změní na **připraven**.</span><span class="sxs-lookup"><span data-stu-id="87014-207">If the status is **In progress**, wait until it is changed to **Ready**.</span></span> <span data-ttu-id="87014-208">Obvykle trvá přibližně **20 minut** k vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="87014-208">It usually takes about **20 minutes** to create an HDInsight cluster.</span></span>

### <a name="check-the-data-factory-output"></a><span data-ttu-id="87014-209">Zkontrolujte výstup objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="87014-209">Check the data factory output</span></span>

1. <span data-ttu-id="87014-210">Stejný postup můžete použijte v poslední relaci ke kontrole kontejnery v kontejneru adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="87014-210">Use the same procedure in the last session to check the containers of the adfgetstarted container.</span></span> <span data-ttu-id="87014-211">Existují dva nové kontejnery kromě **adfgetsarted**:</span><span class="sxs-lookup"><span data-stu-id="87014-211">There are two new containers in addition to **adfgetsarted**:</span></span>

   * <span data-ttu-id="87014-212">Kontejner s názvem, který odpovídá vzorci: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="87014-212">A container with name that follows the pattern: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span></span> <span data-ttu-id="87014-213">Tento kontejner je výchozím kontejnerem pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="87014-213">This container is the default container for the HDInsight cluster.</span></span>
   * <span data-ttu-id="87014-214">adfjobs: Tento kontejner je kontejner pro protokoly úlohy ADF.</span><span class="sxs-lookup"><span data-stu-id="87014-214">adfjobs: This container is the container for the ADF job logs.</span></span>

     <span data-ttu-id="87014-215">Výstup objektu pro vytváření dat je uložen v **afgetstarted** nakonfigurované v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="87014-215">The data factory output is stored in **afgetstarted** as you configured in the Resource Manager template.</span></span>
2. <span data-ttu-id="87014-216">Klikněte na tlačítko **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="87014-216">Click **adfgetstarted**.</span></span>
3. <span data-ttu-id="87014-217">Klikněte dvakrát na **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="87014-217">Double-click **partitioneddata**.</span></span> <span data-ttu-id="87014-218">Zobrazí **rok = 2014** složky protože ze všech webových protokolů v roce 2014.</span><span class="sxs-lookup"><span data-stu-id="87014-218">You see a **year=2014** folder because all the web logs are dated in year 2014.</span></span>

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu výstup](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    <span data-ttu-id="87014-220">Pokud přejdete k podrobnostem v seznamu, zobrazí se tří složek pro leden, února a března.</span><span class="sxs-lookup"><span data-stu-id="87014-220">If you drill down the list, you shall see three folders for January, February, and March.</span></span> <span data-ttu-id="87014-221">A je do protokolu pro každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="87014-221">And there is a log for each month.</span></span>

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu výstup](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-the-template"></a><span data-ttu-id="87014-223">Entity služby Data Factory v šabloně</span><span class="sxs-lookup"><span data-stu-id="87014-223">Data Factory entities in the template</span></span>
<span data-ttu-id="87014-224">Zde je, jak vypadá nejvyšší úrovně šablony Resource Manageru pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="87014-224">Here is how the top-level Resource Manager template for a data factory looks like:</span></span>

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

### <a name="define-data-factory"></a><span data-ttu-id="87014-225">Definování datové továrny</span><span class="sxs-lookup"><span data-stu-id="87014-225">Define data factory</span></span>
<span data-ttu-id="87014-226">Datovou továrnu definujete v šabloně Resource Manageru, jak je znázorněno v následující ukázce:</span><span class="sxs-lookup"><span data-stu-id="87014-226">You define a data factory in the Resource Manager template as shown in the following sample:</span></span>  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
<span data-ttu-id="87014-227">DataFactoryName je název objektu pro vytváření dat, které určíte při nasazování šablony.</span><span class="sxs-lookup"><span data-stu-id="87014-227">The dataFactoryName is the name of the data factory you specify when you deploy the template.</span></span> <span data-ttu-id="87014-228">Objekt pro vytváření dat se aktuálně podporuje jenom v oblasti Východ USA, Severní Evropa a západní USA.</span><span class="sxs-lookup"><span data-stu-id="87014-228">Data factory is currently only supported in the East US, West US, and North Europe regions.</span></span>

### <a name="defining-entities-within-the-data-factory"></a><span data-ttu-id="87014-229">Definování entity v rámci objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="87014-229">Defining entities within the data factory</span></span>
<span data-ttu-id="87014-230">V šabloně JSON jsou definovány následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="87014-230">The following Data Factory entities are defined in the JSON template:</span></span>

* [<span data-ttu-id="87014-231">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="87014-231">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="87014-232">Propojená služba HDInsightu na vyžádání</span><span class="sxs-lookup"><span data-stu-id="87014-232">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="87014-233">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="87014-233">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="87014-234">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="87014-234">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="87014-235">Data Pipeline s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="87014-235">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="87014-236">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="87014-236">Azure Storage linked service</span></span>
<span data-ttu-id="87014-237">Propojená služba AzureStorage propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="87014-237">The Azure Storage linked service links your Azure storage account to the data factory.</span></span> <span data-ttu-id="87014-238">V tomto kurzu se používá stejný účet úložiště jako výchozí účet úložiště HDInsight, úložiště vstupní data a výstupní datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-238">In this tutorial, the same storage account is used as the default HDInsight storage account, input data storage, and output data storage.</span></span> <span data-ttu-id="87014-239">Proto můžete definovat jen jeden Azure Storage, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="87014-239">Therefore, you define only one Azure Storage linked service.</span></span> <span data-ttu-id="87014-240">V definici propojené služby je třeba zadat název a klíč účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="87014-240">In the linked service definition, you specify the name and key of your Azure storage account.</span></span> <span data-ttu-id="87014-241">Podrobnosti o vlastnostech JSON sloužících k definování propojené služby Azure Storage najdete v oddílu [Propojená služba Azure Storage](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service).</span><span class="sxs-lookup"><span data-stu-id="87014-241">See [Azure Storage linked service](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used to define an Azure Storage linked service.</span></span>

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
<span data-ttu-id="87014-242">Vlastnost **connectionString** používá parametry storageAccountName a storageAccountKey.</span><span class="sxs-lookup"><span data-stu-id="87014-242">The **connectionString** uses the storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="87014-243">Zadejte hodnoty pro tyto parametry při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="87014-243">You specify values for these parameters while deploying the template.</span></span>  

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="87014-244">Propojená služba HDInsightu na vyžádání</span><span class="sxs-lookup"><span data-stu-id="87014-244">HDInsight on-demand linked service</span></span>
<span data-ttu-id="87014-245">V definici služby propojené HDInsight na vyžádání zadejte hodnoty pro parametry konfigurace, které jsou používány služba Data Factory k vytvoření clusteru HDInsight Hadoop za běhu.</span><span class="sxs-lookup"><span data-stu-id="87014-245">In the on-demand HDInsight linked service definition, you specify values for configuration parameters that are used by the Data Factory service to create a HDInsight Hadoop cluster at runtime.</span></span> <span data-ttu-id="87014-246">Podrobnosti o vlastnostech JSON používaných k definici propojené služby HDInsightu najdete v článku [Propojené služby Compute](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="87014-246">See [Compute linked services](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used to define an HDInsight on-demand linked service.</span></span>  

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
<span data-ttu-id="87014-247">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="87014-247">Note the following points:</span></span>

* <span data-ttu-id="87014-248">Vytvoří objekt pro vytváření dat **systémem Linux** HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="87014-248">The Data Factory creates a **Linux-based** HDInsight cluster for you.</span></span>
* <span data-ttu-id="87014-249">Vytvoření clusteru HDInsight Hadoop ve stejné oblasti jako účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-249">The HDInsight Hadoop cluster is created in the same region as the storage account.</span></span>
* <span data-ttu-id="87014-250">Upozornění *timeToLive* nastavení.</span><span class="sxs-lookup"><span data-stu-id="87014-250">Notice the *timeToLive* setting.</span></span> <span data-ttu-id="87014-251">Objekt pro vytváření dat cluster odstraní automaticky po clusteru se v nečinnosti, po dobu 30 minut.</span><span class="sxs-lookup"><span data-stu-id="87014-251">The data factory deletes the cluster automatically after the cluster is being idle for 30 minutes.</span></span>
* <span data-ttu-id="87014-252">Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="87014-252">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (**linkedServiceName**).</span></span> <span data-ttu-id="87014-253">Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="87014-253">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="87014-254">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="87014-254">This behavior is by design.</span></span> <span data-ttu-id="87014-255">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je potřeba zpracovat určitý řez, pokud neexistuje aktivní cluster (**timeToLive**), a po dokončení zpracování se zase odstraní.</span><span class="sxs-lookup"><span data-stu-id="87014-255">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs to be processed unless there is an existing live cluster (**timeToLive**) and is deleted when the processing is done.</span></span>

<span data-ttu-id="87014-256">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="87014-256">See [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="87014-257">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="87014-257">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="87014-258">Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-258">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="87014-259">Názvy těchto kontejnerů používají následující formát: „adf**název_vašeho_objektu_pro_vytváření_dat**-**název_propojené_služby**-razítko_data_a_času“.</span><span class="sxs-lookup"><span data-stu-id="87014-259">The names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="87014-260">K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="87014-260">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="87014-261">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="87014-261">Azure blob input dataset</span></span>
<span data-ttu-id="87014-262">V definici vstupní datové sady zadejte názvy kontejneru objektů blob, složku a soubor, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="87014-262">In the input dataset definition, you specify the names of blob container, folder, and file that contains the input data.</span></span> <span data-ttu-id="87014-263">Podrobnosti o vlastnostech JSON sloužících k definování datové sady Azure Blob najdete v oddílu [Vlastnosti datové sady Azure Blob](../data-factory/data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="87014-263">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span>

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

<span data-ttu-id="87014-264">Všimněte si následujících konkrétní nastavení v definici JSON:</span><span class="sxs-lookup"><span data-stu-id="87014-264">Notice the following specific settings in the JSON definition:</span></span>

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="87014-265">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="87014-265">Azure Blob output dataset</span></span>
<span data-ttu-id="87014-266">V definici výstupní datovou sadu zadejte názvy kontejneru objektů blob a složky, která obsahuje výstupní data.</span><span class="sxs-lookup"><span data-stu-id="87014-266">In the output dataset definition, you specify the names of blob container and folder that holds the output data.</span></span> <span data-ttu-id="87014-267">Podrobnosti o vlastnostech JSON sloužících k definování datové sady Azure Blob najdete v oddílu [Vlastnosti datové sady Azure Blob](../data-factory/data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="87014-267">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span>  

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

<span data-ttu-id="87014-268">FolderPath Určuje cestu ke složce, která obsahuje výstupní data:</span><span class="sxs-lookup"><span data-stu-id="87014-268">The folderPath specifies the path to the folder that holds the output data:</span></span>

```json
"folderPath": "adfgetstarted/partitioneddata",
```

<span data-ttu-id="87014-269">[Datovou sadu dostupnosti](../data-factory/data-factory-create-datasets.md#dataset-availability) nastavení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="87014-269">The [dataset availability](../data-factory/data-factory-create-datasets.md#dataset-availability) setting is as follows:</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

<span data-ttu-id="87014-270">V Azure Data Factory výstupní datovou sadu dostupnosti jednotky kanálu.</span><span class="sxs-lookup"><span data-stu-id="87014-270">In Azure Data Factory, output dataset availability drives the pipeline.</span></span> <span data-ttu-id="87014-271">V tomto příkladu je řez vytváří jednou měsíčně poslední den v měsíci (EndOfInterval).</span><span class="sxs-lookup"><span data-stu-id="87014-271">In this example, the slice is produced monthly on the last day of month (EndOfInterval).</span></span> <span data-ttu-id="87014-272">Další informace najdete v tématu [Data Factory plánování a provádění](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="87014-272">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

#### <a name="data-pipeline"></a><span data-ttu-id="87014-273">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="87014-273">Data pipeline</span></span>
<span data-ttu-id="87014-274">Můžete definovat kanál, který transformuje data pomocí skriptu Hive v clusteru Azure HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="87014-274">You define a pipeline that transforms data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="87014-275">Popisy elementů JSON sloužících k definování kanálu v tomto příkladu najdete v oddílu [Kód JSON kanálu](../data-factory/data-factory-create-pipelines.md#pipeline-json).</span><span class="sxs-lookup"><span data-stu-id="87014-275">See [Pipeline JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used to define a pipeline in this example.</span></span>

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

<span data-ttu-id="87014-276">Kanál obsahuje jednu aktivitu, aktivita HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="87014-276">The pipeline contains one activity, HDInsightHive activity.</span></span> <span data-ttu-id="87014-277">Jako počáteční a koncová data jsou v leden 2016, data pro pouze jeden měsíc () zpracování řezu se.</span><span class="sxs-lookup"><span data-stu-id="87014-277">As both start and end dates are in January 2016, data for only one month (a slice) is processed.</span></span> <span data-ttu-id="87014-278">Obě *spustit* a *end* aktivity mají na datum v minulosti, takže objektu pro vytváření dat zpracovává data pro daný měsíc okamžitě.</span><span class="sxs-lookup"><span data-stu-id="87014-278">Both *start* and *end* of the activity have a past date, so the Data Factory processes data for the month immediately.</span></span> <span data-ttu-id="87014-279">Pokud element end budoucí datum, data factory vytvoří jiné řez, když nastane čas.</span><span class="sxs-lookup"><span data-stu-id="87014-279">If the end is a future date, the data factory creates another slice when the time comes.</span></span> <span data-ttu-id="87014-280">Další informace najdete v tématu [Data Factory plánování a provádění](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="87014-280">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

## <a name="clean-up-the-tutorial"></a><span data-ttu-id="87014-281">Vyčistěte kurz</span><span class="sxs-lookup"><span data-stu-id="87014-281">Clean up the tutorial</span></span>

### <a name="delete-the-blob-containers-created-by-on-demand-hdinsight-cluster"></a><span data-ttu-id="87014-282">Odstranění kontejnerů objektů blob, vytvořit cluster HDInsight na vyžádání</span><span class="sxs-lookup"><span data-stu-id="87014-282">Delete the blob containers created by on-demand HDInsight cluster</span></span>
<span data-ttu-id="87014-283">S na vyžádání propojené služby HDInsight HDInsight cluster vytvoří pokaždé, když řez je potřeba zpracovat, pokud neexistuje aktivní cluster (timeToLive); a cluster bude odstraněn, pokud se provádí zpracování.</span><span class="sxs-lookup"><span data-stu-id="87014-283">With on-demand HDInsight linked service, an HDInsight cluster is created every time a slice needs to be processed unless there is an existing live cluster (timeToLive); and the cluster is deleted when the processing is done.</span></span> <span data-ttu-id="87014-284">Pro každý cluster Azure Data Factory vytvoří kontejner objektů blob v Azure blob storage používá jako výchozí účet stroage pro cluster.</span><span class="sxs-lookup"><span data-stu-id="87014-284">For each cluster, Azure Data Factory creates a blob container in the Azure blob storage used as the default stroage account for the cluster.</span></span> <span data-ttu-id="87014-285">I když dojde k odstranění clusteru HDInsight, nebudou odstraněny výchozího kontejneru blob storage a přidruženého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-285">Even though HDInsight cluster is deleted, the default blob storage container and the associated storage account are not deleted.</span></span> <span data-ttu-id="87014-286">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="87014-286">This behavior is by design.</span></span> <span data-ttu-id="87014-287">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="87014-287">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="87014-288">Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-288">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="87014-289">Názvy těchto kontejnerů se řídí vzorem: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="87014-289">The names of these containers follow a pattern: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span></span>

<span data-ttu-id="87014-290">Odstranit **adfjobs** a **adfyourdatafactoryname-linkedservicename razítko_data_a_času** složky.</span><span class="sxs-lookup"><span data-stu-id="87014-290">Delete the **adfjobs** and **adfyourdatafactoryname-linkedservicename-datetimestamp** folders.</span></span> <span data-ttu-id="87014-291">Kontejner adfjobs obsahuje protokoly úlohy Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="87014-291">The adfjobs container contains job logs from Azure Data Factory.</span></span>

### <a name="delete-the-resource-group"></a><span data-ttu-id="87014-292">Odstraňte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="87014-292">Delete the resource group</span></span>
<span data-ttu-id="87014-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) se používá k nasazení, správě a monitorování vašeho řešení jako se skupinou.</span><span class="sxs-lookup"><span data-stu-id="87014-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) is used to deploy, manage, and monitor your solution as a group.</span></span>  <span data-ttu-id="87014-294">Odstranění skupiny prostředků se odstraní všechny součásti ve skupině.</span><span class="sxs-lookup"><span data-stu-id="87014-294">Deleting a resource group deletes all the components inside the group.</span></span>  

1. <span data-ttu-id="87014-295">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="87014-295">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="87014-296">Klikněte na tlačítko **skupiny prostředků** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="87014-296">Click **Resource groups** on the left pane.</span></span>
3. <span data-ttu-id="87014-297">Klikněte na název skupiny prostředků, kterou jste vytvořili ve vašem skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87014-297">Click the resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="87014-298">Pokud máte příliš mnoho skupin prostředků, které jsou uvedeny, použijte filtr.</span><span class="sxs-lookup"><span data-stu-id="87014-298">Use the filter if you have too many resource groups listed.</span></span> <span data-ttu-id="87014-299">Skupina prostředků se otevře v novém okně.</span><span class="sxs-lookup"><span data-stu-id="87014-299">It opens the resource group in a new blade.</span></span>
4. <span data-ttu-id="87014-300">Na **prostředky** dlaždice, musí mít výchozí účet úložiště a objektu pro vytváření dat uvedené Pokud sdílíte s jinými projekty skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="87014-300">On the **Resources** tile, you shall have the default storage account and the data factory listed unless you share the resource group with other projects.</span></span>
5. <span data-ttu-id="87014-301">Klikněte na tlačítko **odstranit** nahoře v okně.</span><span class="sxs-lookup"><span data-stu-id="87014-301">Click **Delete** on the top of the blade.</span></span> <span data-ttu-id="87014-302">Díky tomu odstraní účet úložiště a data uložená v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-302">Doing so deletes the storage account and the data stored in the storage account.</span></span>
6. <span data-ttu-id="87014-303">Zadejte název skupiny prostředků Potvrdit odstranění, a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="87014-303">Enter the resource group name to confirm deletion, and then click **Delete**.</span></span>

<span data-ttu-id="87014-304">V případě, že nechcete odstranit účet úložiště, když odstraníte skupinu prostředků, zvažte následující architektura oddělením firemních dat z výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="87014-304">In case you don't want to delete the storage account when you delete the resource group, consider the following architecture by separating the business data from the default storage account.</span></span> <span data-ttu-id="87014-305">V takovém případě je třeba jedna skupina prostředků pro účet úložiště s daty obchodních a jiné skupině prostředků pro výchozí účet úložiště pro HDInsight propojené služby a služby data factory.</span><span class="sxs-lookup"><span data-stu-id="87014-305">In this case, you have one resource group for the storage account with the business data, and the other resource group for the default storage account for HDInsight linked service and the data factory.</span></span> <span data-ttu-id="87014-306">Pokud odstraníte druhé skupině prostředků, neovlivní účet úložiště obchodní data.</span><span class="sxs-lookup"><span data-stu-id="87014-306">When you delete the second resource group, it does not impact the business data storage account.</span></span> <span data-ttu-id="87014-307">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="87014-307">To do so:</span></span>

* <span data-ttu-id="87014-308">Přidejte následující ke skupině prostředků nejvyšší úrovně společně s Microsoft.DataFactory/datafactories prostředků ve vaší šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="87014-308">Add the following to the top-level resource group along with the Microsoft.DataFactory/datafactories resource in your Resource Manager template.</span></span> <span data-ttu-id="87014-309">Vytvoří účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="87014-309">It creates a storage account:</span></span>

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
* <span data-ttu-id="87014-310">Přidejte nový bod propojené služby do nového účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="87014-310">Add a new linked service point to the new storage account:</span></span>

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
* <span data-ttu-id="87014-311">Nakonfigurujte další dependsOn a additionalLinkedServiceNames ondemand propojená služba HDInsight:</span><span class="sxs-lookup"><span data-stu-id="87014-311">Configure the HDInsight ondemand linked service with an additional dependsOn and an additionalLinkedServiceNames:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="87014-312">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87014-312">Next steps</span></span>
<span data-ttu-id="87014-313">V tomto článku jste se naučili, jak používat Azure Data Factory k vytvoření clusteru HDInsight na vyžádání ke zpracování úloh Hive.</span><span class="sxs-lookup"><span data-stu-id="87014-313">In this article, you have learned how to use Azure Data Factory to create on-demand HDInsight cluster to process Hive jobs.</span></span> <span data-ttu-id="87014-314">Další informace:</span><span class="sxs-lookup"><span data-stu-id="87014-314">To read more:</span></span>

* [<span data-ttu-id="87014-315">Kurz Hadoopu: začněte používat systémem Linux Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="87014-315">Hadoop tutorial: Get started using Linux-based Hadoop in HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="87014-316">Vytvořit clustery se systémem Linux Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="87014-316">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="87014-317">Dokumentace prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="87014-317">HDInsight documentation</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="87014-318">Dokumentace k objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="87014-318">Data factory documentation</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a><span data-ttu-id="87014-319">Příloha</span><span class="sxs-lookup"><span data-stu-id="87014-319">Appendix</span></span>

### <a name="azure-cli-script"></a><span data-ttu-id="87014-320">Azure CLI skriptu</span><span class="sxs-lookup"><span data-stu-id="87014-320">Azure CLI script</span></span>
<span data-ttu-id="87014-321">Místo použití prostředí Azure PowerShell udělat tento kurz můžete použít rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="87014-321">You can use Azure CLI instead of using Azure PowerShell to do the tutorial.</span></span> <span data-ttu-id="87014-322">Chcete-li používat rozhraní příkazového řádku Azure, nejprve nainstalujte rozhraní příkazového řádku Azure podle následujících pokynů:</span><span class="sxs-lookup"><span data-stu-id="87014-322">To use Azure CLI, first install Azure CLI as per the following instructions:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-to-prepare-the-storage-and-copy-the-files"></a><span data-ttu-id="87014-323">Příprava úložiště a zkopírujte soubory pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="87014-323">Use Azure CLI to prepare the storage and copy the files</span></span>

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

<span data-ttu-id="87014-324">Název kontejneru je *adfgetstarted*.</span><span class="sxs-lookup"><span data-stu-id="87014-324">The container name is *adfgetstarted*.</span></span> <span data-ttu-id="87014-325">Protože se jedná, uchovávejte ho.</span><span class="sxs-lookup"><span data-stu-id="87014-325">Keep it as it is.</span></span> <span data-ttu-id="87014-326">V opačném případě je potřeba aktualizovat šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="87014-326">Otherwise you need to update the Resource Manager template.</span></span> <span data-ttu-id="87014-327">Pokud potřebujete pomoc s Tento skript rozhraní příkazového řádku, přečtěte si [použití Azure CLI s Azure Storage](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="87014-327">If you need help with this CLI script, see [Using the Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>
