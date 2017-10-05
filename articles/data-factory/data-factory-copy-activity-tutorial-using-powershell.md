---
title: "Kurz: Vytvoření kanálu pro přesouvání dat pomocí Azure PowerShellu | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí Azure PowerShellu."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 81efe7c6af29af778686e1f6bcf62fedc9711052
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a><span data-ttu-id="d0a49-103">Kurz: Vytvoření kanálu Data Factory pro přesouvání dat pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="d0a49-103">Tutorial: Create a Data Factory pipeline that moves data by using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d0a49-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="d0a49-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="d0a49-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="d0a49-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="d0a49-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d0a49-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="d0a49-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0a49-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="d0a49-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0a49-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="d0a49-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d0a49-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="d0a49-110">REST API</span><span class="sxs-lookup"><span data-stu-id="d0a49-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="d0a49-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="d0a49-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

<span data-ttu-id="d0a49-112">V tomto článku se naučíte, jak používat PowerShell, abyste vytvořili datovou továrnu s kanálem, který kopíruje data z úložiště objektů blob v Azure do databáze SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-112">In this article, you learn how to use PowerShell to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="d0a49-113">Pokud s Azure Data Factory začínáte, přečtěte si článek [Seznámení se službou Azure Data Factory](data-factory-introduction.md), než s tímto kurzem začnete.</span><span class="sxs-lookup"><span data-stu-id="d0a49-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="d0a49-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="d0a49-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="d0a49-115">Aktivita kopírování kopíruje data z podporovaného úložiště dat do podporovaného úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="d0a49-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="d0a49-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="d0a49-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="d0a49-117">Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="d0a49-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="d0a49-118">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="d0a49-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="d0a49-120">A dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="d0a49-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="d0a49-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="d0a49-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="d0a49-122">Tento článek nepopisuje všechny rutiny služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d0a49-122">This article does not cover all the Data Factory cmdlets.</span></span> <span data-ttu-id="d0a49-123">Úplnou dokumentaci k těmto rutinám najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).</span><span class="sxs-lookup"><span data-stu-id="d0a49-123">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on these cmdlets.</span></span>
> 
> <span data-ttu-id="d0a49-124">Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="d0a49-124">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="d0a49-125">Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-125">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0a49-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d0a49-126">Prerequisites</span></span>
- <span data-ttu-id="d0a49-127">Dokončete požadované kroky uvedené v [požadavcích kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-127">Complete prerequisites listed in the [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article.</span></span>
- <span data-ttu-id="d0a49-128">Nainstalujte **Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-128">Install **Azure PowerShell**.</span></span> <span data-ttu-id="d0a49-129">Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-129">Follow the instructions in [How to install and configure Azure PowerShell](../powershell-install-configure.md).</span></span>

## <a name="steps"></a><span data-ttu-id="d0a49-130">Kroky</span><span class="sxs-lookup"><span data-stu-id="d0a49-130">Steps</span></span>
<span data-ttu-id="d0a49-131">Zde jsou kroky, které provedete v rámci tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="d0a49-131">Here are the steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="d0a49-132">Vytvořte **datovou továrnu** Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-132">Create an Azure **data factory**.</span></span> <span data-ttu-id="d0a49-133">V tomto kroku vytvoříte datovou továrnu s názvem ADFTutorialDataFactoryPSH.</span><span class="sxs-lookup"><span data-stu-id="d0a49-133">In this step, you create a data factory named ADFTutorialDataFactoryPSH.</span></span> 
2. <span data-ttu-id="d0a49-134">V této datové továrně vytvořte **propojené služby**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-134">Create **linked services** in the data factory.</span></span> <span data-ttu-id="d0a49-135">V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d0a49-135">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="d0a49-136">Služba AzureStorageLinkedService propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="d0a49-136">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="d0a49-137">V rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) jste vytvořili kontejner a nahráli data do tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d0a49-137">You created a container and uploaded data to this storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="d0a49-138">Služba AzureSqlLinkedService propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="d0a49-138">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="d0a49-139">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="d0a49-139">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="d0a49-140">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.</span><span class="sxs-lookup"><span data-stu-id="d0a49-140">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="d0a49-141">Vytvořte v datové továrně vstupní a výstupní **datové sady**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-141">Create input and output **datasets** in the data factory.</span></span>  
    
    <span data-ttu-id="d0a49-142">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-142">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="d0a49-143">A vstupní datová sada objektu blob určuje kontejner a složku obsahující vstupní data.</span><span class="sxs-lookup"><span data-stu-id="d0a49-143">And, the input blob dataset specifies the container and the folder that contains the input data.</span></span>  

    <span data-ttu-id="d0a49-144">Podobně také propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="d0a49-144">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="d0a49-145">A výstupní datová sada tabulky SQL určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d0a49-145">And, the output SQL table dataset specifies the table in the database to which the data from the blob storage is copied.</span></span>
4. <span data-ttu-id="d0a49-146">Vytvořte v datové továrně **kanál**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-146">Create a **pipeline** in the data factory.</span></span> <span data-ttu-id="d0a49-147">V tomto kroku pomocí aktivity kopírování vytvoříte kanál.</span><span class="sxs-lookup"><span data-stu-id="d0a49-147">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="d0a49-148">Aktivita kopírování kopíruje data z objektu blob v úložišti objektů blob v Azure do tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="d0a49-148">The copy activity copies data from a blob in the Azure blob storage to a table in the Azure SQL database.</span></span> <span data-ttu-id="d0a49-149">Aktivitu kopírování můžete v kanálu použít ke kopírování dat z jakéhokoli podporovaného zdroje do jakéhokoli podporovaného cíle.</span><span class="sxs-lookup"><span data-stu-id="d0a49-149">You can use a copy activity in a pipeline to copy data from any supported source to any supported destination.</span></span> <span data-ttu-id="d0a49-150">Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="d0a49-150">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="d0a49-151">Monitorujte kanál.</span><span class="sxs-lookup"><span data-stu-id="d0a49-151">Monitor the pipeline.</span></span> <span data-ttu-id="d0a49-152">V tomto kroku budete **monitorovat** řezy vstupních a výstupních datových sad pomocí PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-152">In this step, you **monitor** the slices of input and output datasets by using PowerShell.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="d0a49-153">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="d0a49-153">Create a data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d0a49-154">Pokud jste tak ještě neučinili, dokončete [požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-154">Complete [prerequisites for the tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="d0a49-155">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="d0a49-155">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="d0a49-156">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="d0a49-156">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="d0a49-157">Může obsahovat například aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu HDInsight Hive pro spuštění skriptu Hive, který umožňuje transformovat vstupní data na výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-157">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="d0a49-158">V tomto kroku začneme vytvořením objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="d0a49-158">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="d0a49-159">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-159">Launch **PowerShell**.</span></span> <span data-ttu-id="d0a49-160">Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-160">Keep Azure PowerShell open until the end of this tutorial.</span></span> <span data-ttu-id="d0a49-161">Pokud ho zavřete a znovu otevřete, bude potřeba tyto příkazy spustit znovu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-161">If you close and reopen, you need to run the commands again.</span></span>

    <span data-ttu-id="d0a49-162">Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="d0a49-162">Run the following command, and enter the user name and password that you use to sign in to the Azure portal:</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    <span data-ttu-id="d0a49-163">Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet:</span><span class="sxs-lookup"><span data-stu-id="d0a49-163">Run the following command to view all the subscriptions for this account:</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="d0a49-164">Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat.</span><span class="sxs-lookup"><span data-stu-id="d0a49-164">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="d0a49-165">Místo **&lt;NameOfAzureSubscription**&gt; uveďte název svého předplatného Azure:</span><span class="sxs-lookup"><span data-stu-id="d0a49-165">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription:</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. <span data-ttu-id="d0a49-166">Spuštěním následujícího příkazu vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="d0a49-166">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    <span data-ttu-id="d0a49-167">Některé kroky v tomto kurzu vychází z předpokladu, že používáte skupinu prostředků s názvem **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-167">Some of the steps in this tutorial assume that you use the resource group named **ADFTutorialResourceGroup**.</span></span> <span data-ttu-id="d0a49-168">Pokud máte jinou skupinu prostředků, použijte ji v postupech v tomto kurzu místo skupiny ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="d0a49-168">If you use a different resource group, you need to use it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="d0a49-169">Spuštěním rutiny **New-AzureRmDataFactory** vytvořte datovou továrnu s názvem **ADFTutorialDataFactoryPSH**:</span><span class="sxs-lookup"><span data-stu-id="d0a49-169">Run the **New-AzureRmDataFactory** cmdlet to create a data factory named **ADFTutorialDataFactoryPSH**:</span></span>  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    <span data-ttu-id="d0a49-170">Tento název už nejspíš není volný.</span><span class="sxs-lookup"><span data-stu-id="d0a49-170">This name may already have been taken.</span></span> <span data-ttu-id="d0a49-171">Vytvořte proto jedinečný název datové továrny přidáním předpony nebo přípony (například: ADFTutorialDataFactoryPSH05152017) a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-171">Therefore, make the name of the data factory unique by adding a prefix or suffix (for example: ADFTutorialDataFactoryPSH05152017) and run the command again.</span></span>  

<span data-ttu-id="d0a49-172">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="d0a49-172">Note the following points:</span></span>

* <span data-ttu-id="d0a49-173">Název objektu pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="d0a49-173">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="d0a49-174">Pokud se zobrazí následující chyba, změňte název (třeba na váš_název_ADFTutorialDataFactoryPSH).</span><span class="sxs-lookup"><span data-stu-id="d0a49-174">If you receive the following error, change the name (for example, yournameADFTutorialDataFactoryPSH).</span></span> <span data-ttu-id="d0a49-175">Při provádění postupů v tomto kurzu potom tento název používejte místo názvu ADFTutorialFactoryPSH.</span><span class="sxs-lookup"><span data-stu-id="d0a49-175">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="d0a49-176">Informace o artefaktech služby Data Factory najdete v tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-176">See [Data Factory - Naming Rules](data-factory-naming-rules.md) for Data Factory artifacts.</span></span>

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* <span data-ttu-id="d0a49-177">Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem nebo správcem předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-177">To create Data Factory instances, you must be a contributor or administrator of the Azure subscription.</span></span>
* <span data-ttu-id="d0a49-178">Název datové továrny se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="d0a49-178">The name of the data factory may be registered as a DNS name in the future, and hence become publicly visible.</span></span>
* <span data-ttu-id="d0a49-179">Mohla se zobrazit tato chyba: **Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory.**</span><span class="sxs-lookup"><span data-stu-id="d0a49-179">You may receive the following error: "**This subscription is not registered to use namespace Microsoft.DataFactory.**"</span></span> <span data-ttu-id="d0a49-180">Proveďte jednu z následujících a zkuste publikování znovu:</span><span class="sxs-lookup"><span data-stu-id="d0a49-180">Do one of the following, and try publishing again:</span></span>

  * <span data-ttu-id="d0a49-181">Spuštěním následujícího příkazu v prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="d0a49-181">In Azure PowerShell, run the following command to register the Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    <span data-ttu-id="d0a49-182">Spuštěním následujícího příkazu ověřte, zda je zprostředkovatel služby Data Factory zaregistrovaný:</span><span class="sxs-lookup"><span data-stu-id="d0a49-182">Run the following command to confirm that the Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="d0a49-183">Přihlaste se pomocí předplatného Azure k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d0a49-183">Sign in by using the Azure subscription to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d0a49-184">Přejděte do okna Data Factory, nebo vytvořte datovou továrnu na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="d0a49-184">Go to a Data Factory blade, or create a data factory in the Azure portal.</span></span> <span data-ttu-id="d0a49-185">Zprostředkovatel se při takovém postupu zaregistruje automaticky.</span><span class="sxs-lookup"><span data-stu-id="d0a49-185">This action automatically registers the provider for you.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="d0a49-186">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="d0a49-186">Create linked services</span></span>
<span data-ttu-id="d0a49-187">V datové továrně vytvoříte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="d0a49-187">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="d0a49-188">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="d0a49-188">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="d0a49-189">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="d0a49-189">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="d0a49-190">Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="d0a49-190">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="d0a49-191">Služba AzureStorageLinkedService propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="d0a49-191">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="d0a49-192">Tento účet úložiště je ten, ve kterém jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili kontejner a nahráli do něj data.</span><span class="sxs-lookup"><span data-stu-id="d0a49-192">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="d0a49-193">Služba AzureSqlLinkedService propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="d0a49-193">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="d0a49-194">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="d0a49-194">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="d0a49-195">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku emp.</span><span class="sxs-lookup"><span data-stu-id="d0a49-195">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="d0a49-196">Vytvoření propojené služby pro účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="d0a49-196">Create a linked service for an Azure storage account</span></span>
<span data-ttu-id="d0a49-197">V tomto kroku propojíte se svou datovou továrnou účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-197">In this step, you link your Azure storage account to your data factory.</span></span>

1. <span data-ttu-id="d0a49-198">Vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** ve složce **C:\ADFGetStartedPSH** s následujícím obsahem: (pokud ještě neexistuje, složku ADFGetStartedPSH vytvořte).</span><span class="sxs-lookup"><span data-stu-id="d0a49-198">Create a JSON file named **AzureStorageLinkedService.json** in **C:\ADFGetStartedPSH** folder with the following content: (Create the folder ADFGetStartedPSH if it does not already exist.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d0a49-199">Než soubor uložíte, položky &lt;accountname&gt; a &lt;accountkey&gt; nahraďte názvem svého účtu Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="d0a49-199">Replace &lt;accountname&gt; and &lt;accountkey&gt; with name and key of your Azure storage account before saving the file.</span></span> 

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
     }
    ``` 
2. <span data-ttu-id="d0a49-200">V prostředí **Azure PowerShell** přejděte do složky **ADFGetStartedPSH**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-200">In **Azure PowerShell**, switch to the **ADFGetStartedPSH** folder.</span></span>
4. <span data-ttu-id="d0a49-201">Spuštěním rutiny **New-AzureRmDataFactoryLinkedService** vytvořte propojenou službu **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-201">Run the **New-AzureRmDataFactoryLinkedService** cmdlet to create the linked service: **AzureStorageLinkedService**.</span></span> <span data-ttu-id="d0a49-202">Tato rutina a další rutiny služby Data Factory používané v tomto kurzu vyžadují, abyste zadali hodnoty parametrů **ResourceGroupName** a **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-202">This cmdlet, and other Data Factory cmdlets you use in this tutorial requires you to pass values for the **ResourceGroupName** and **DataFactoryName** parameters.</span></span> <span data-ttu-id="d0a49-203">Alternativně můžete objekt DataFactory vrácený rutinou New-AzureRmDataFactory předat, abyste nemuseli při každém spouštění rutiny zadávat hodnoty parametrů ResourceGroupName a DataFactoryName.</span><span class="sxs-lookup"><span data-stu-id="d0a49-203">Alternatively, you can pass the DataFactory object returned by the New-AzureRmDataFactory cmdlet without typing ResourceGroupName and DataFactoryName each time you run a cmdlet.</span></span> 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    <span data-ttu-id="d0a49-204">Zde je ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="d0a49-204">Here is the sample output:</span></span>

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    <span data-ttu-id="d0a49-205">Druhým způsobem vytvoření této propojené služby je zadání názvu skupiny prostředků a názvu datové továrny místo zadání objektu DataFactory.</span><span class="sxs-lookup"><span data-stu-id="d0a49-205">Other way of creating this linked service is to specify resource group name and data factory name instead of specifying the DataFactory object.</span></span>  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a><span data-ttu-id="d0a49-206">Vytvoření propojené služby pro Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="d0a49-206">Create a linked service for an Azure SQL database</span></span>
<span data-ttu-id="d0a49-207">V tomto kroku se svým objektem pro vytváření dat propojíte svou databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-207">In this step, you link your Azure SQL database to your data factory.</span></span>

1. <span data-ttu-id="d0a49-208">Ve složce C:\ADFGetStartedPSH vytvořte soubor JSON s názvem AzureSqlLinkedService.json s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d0a49-208">Create a JSON file named AzureSqlLinkedService.json in C:\ADFGetStartedPSH folder with the following content:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d0a49-209">Položky &lt;název_serveru&gt;, &lt;název_databáze&gt;, &lt;username@servername&gt; a &lt;heslo&gt; nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským účtem a heslem.</span><span class="sxs-lookup"><span data-stu-id="d0a49-209">Replace &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt;, and &lt;password&gt; with names of your Azure SQL server, database, user account, and password.</span></span>
    
    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
2. <span data-ttu-id="d0a49-210">Spuštěním následujícího příkazu vytvořte propojenou službu:</span><span class="sxs-lookup"><span data-stu-id="d0a49-210">Run the following command to create a linked service:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    <span data-ttu-id="d0a49-211">Zde je ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="d0a49-211">Here is the sample output:</span></span>

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   <span data-ttu-id="d0a49-212">Ujistěte se, že nastavení **Povolit přístup ke službám Azure** je pro server SQL Database zapnuté.</span><span class="sxs-lookup"><span data-stu-id="d0a49-212">Confirm that **Allow access to Azure services** setting is turned on for your SQL database server.</span></span> <span data-ttu-id="d0a49-213">Chcete-li to ověřit a zapnout ho, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d0a49-213">To verify and turn it on, do the following steps:</span></span>

    1. <span data-ttu-id="d0a49-214">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d0a49-214">Log in to the [Azure portal](https://portal.azure.com)</span></span>
    2. <span data-ttu-id="d0a49-215">Vlevo klikněte na možnost **Další služby** a potom na **Servery SQL** v kategorii **DATABÁZE**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-215">Click **More services >** on the left, and click **SQL servers** in the **DATABASES** category.</span></span>
    3. <span data-ttu-id="d0a49-216">Ze seznamu serverů SQL vyberte svůj server.</span><span class="sxs-lookup"><span data-stu-id="d0a49-216">Select your server in the list of SQL servers.</span></span>
    4. <span data-ttu-id="d0a49-217">V okně Server SQL klikněte na odkaz **Zobrazit nastavení brány firewall**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-217">On the SQL server blade, click **Show firewall settings** link.</span></span>
    5. <span data-ttu-id="d0a49-218">V okně **Nastavení brány firewall** klikněte na **ZAPNUTO** u možnosti **Povolit přístup ke službám Azure**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-218">In the **Firewall settings** blade, click **ON** for **Allow access to Azure services**.</span></span>
    6. <span data-ttu-id="d0a49-219">Na panelu nástrojů klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-219">Click **Save** on the toolbar.</span></span> 

## <a name="create-datasets"></a><span data-ttu-id="d0a49-220">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="d0a49-220">Create datasets</span></span>
<span data-ttu-id="d0a49-221">V předchozím kroku jste vytvořili propojené služby, abyste propojili účet úložiště Azure a Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="d0a49-221">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="d0a49-222">V tomto kroku nadefinujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data uložená v úložištích dat, na která odkazují služby AzureStorageLinkedService a AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="d0a49-222">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="d0a49-223">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-223">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="d0a49-224">A vstupní datová sada objektu blob (InputDataset) určuje kontejner a složku obsahující vstupní data.</span><span class="sxs-lookup"><span data-stu-id="d0a49-224">And, the input blob dataset (InputDataset) specifies the container and the folder that contains the input data.</span></span>  

<span data-ttu-id="d0a49-225">Podobně také propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="d0a49-225">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="d0a49-226">A výstupní datová sada tabulky SQL (OutputDataset) určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d0a49-226">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span> 

### <a name="create-an-input-dataset"></a><span data-ttu-id="d0a49-227">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="d0a49-227">Create an input dataset</span></span>
<span data-ttu-id="d0a49-228">V tomto kroku vytvoříte datovou sadu s názvem InputDataset, která odkazuje na soubor blob (emp.txt) v kořenové složce kontejneru objektů blob (adftutorial), který se nachází ve službě Azure Storage reprezentované propojenou službou AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="d0a49-228">In this step, you create a dataset named InputDataset that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="d0a49-229">Pokud neurčíte hodnotu fileName (nebo ji přeskočíte), data ze všech objektů blob ve vstupní složce se zkopírují do cíle.</span><span class="sxs-lookup"><span data-stu-id="d0a49-229">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="d0a49-230">V tomto kurzu určíte hodnotu fileName.</span><span class="sxs-lookup"><span data-stu-id="d0a49-230">In this tutorial, you specify a value for the fileName.</span></span>  

1. <span data-ttu-id="d0a49-231">Ve složce **C:\ADFGetStartedPSH** vytvořte soubor JSON s názvem **InputDataset.json** s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d0a49-231">Create a JSON file named **InputDataset.json** in the **C:\ADFGetStartedPSH** folder, with the following content:</span></span>

    ```json
    {
        "name": "InputDataset",
        "properties": {
            "structure": [
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "emp.txt",
                "folderPath": "adftutorial/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```

    <span data-ttu-id="d0a49-232">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="d0a49-232">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="d0a49-233">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="d0a49-233">Property</span></span> | <span data-ttu-id="d0a49-234">Popis</span><span class="sxs-lookup"><span data-stu-id="d0a49-234">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="d0a49-235">type</span><span class="sxs-lookup"><span data-stu-id="d0a49-235">type</span></span> | <span data-ttu-id="d0a49-236">Vlastnost type je nastavená na hodnotu **AzureBlob**, protože se data nachází ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="d0a49-236">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="d0a49-237">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="d0a49-237">linkedServiceName</span></span> | <span data-ttu-id="d0a49-238">Odkazuje na službu **AzureStorageLinkedService**, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="d0a49-238">Refers to the **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="d0a49-239">folderPath</span><span class="sxs-lookup"><span data-stu-id="d0a49-239">folderPath</span></span> | <span data-ttu-id="d0a49-240">Určuje **kontejner** objektů blob a **složku** obsahující vstupní objekty blob.</span><span class="sxs-lookup"><span data-stu-id="d0a49-240">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> <span data-ttu-id="d0a49-241">V tomto kurzu je adftutorial kontejnerem objektů blob a složka je kořenová složka.</span><span class="sxs-lookup"><span data-stu-id="d0a49-241">In this tutorial, adftutorial is the blob container and folder is the root folder.</span></span> | 
    | <span data-ttu-id="d0a49-242">fileName</span><span class="sxs-lookup"><span data-stu-id="d0a49-242">fileName</span></span> | <span data-ttu-id="d0a49-243">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="d0a49-243">This property is optional.</span></span> <span data-ttu-id="d0a49-244">Pokud ji vynecháte, vyberou se všechny soubory v cestě folderPath.</span><span class="sxs-lookup"><span data-stu-id="d0a49-244">If you omit this property, all files from the folderPath are picked.</span></span> <span data-ttu-id="d0a49-245">V tomto kurzu má fileName hodnotu **emp.txt**, takže se zpracuje pouze tento soubor.</span><span class="sxs-lookup"><span data-stu-id="d0a49-245">In this tutorial, **emp.txt** is specified for the fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="d0a49-246">format -> type</span><span class="sxs-lookup"><span data-stu-id="d0a49-246">format -> type</span></span> |<span data-ttu-id="d0a49-247">Vstupní soubor je v textovém formátu, takže použijeme **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-247">The input file is in the text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="d0a49-248">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="d0a49-248">columnDelimiter</span></span> | <span data-ttu-id="d0a49-249">Sloupce ve vstupním souboru jsou oddělené **znakem čárky (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-249">The columns in the input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="d0a49-250">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="d0a49-250">frequency/interval</span></span> | <span data-ttu-id="d0a49-251">Frekvence je nastavená na hodnotu **Hour** (hodina) a interval je **1**, takže vstupní řezy jsou dostupné **každou hodinu**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-251">The frequency is set to **Hour** and interval is  set to **1**, which means that the input slices are available **hourly**.</span></span> <span data-ttu-id="d0a49-252">Jinými slovy služba Data Factory každou hodinu vyhledá vstupní data v kořenové složce kontejneru objektů blob (**adftutorial**), který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="d0a49-252">In other words, the Data Factory service looks for input data every hour in the root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="d0a49-253">Vyhledává data v rámci kanálu mezi časy spuštění a ukončení, ne před nebo po této době.</span><span class="sxs-lookup"><span data-stu-id="d0a49-253">It looks for the data within the pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="d0a49-254">external</span><span class="sxs-lookup"><span data-stu-id="d0a49-254">external</span></span> | <span data-ttu-id="d0a49-255">Pokud data nevygeneroval tento kanál, je tato vlastnost nastavená na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-255">This property is set to **true** if the data is not generated by this pipeline.</span></span> <span data-ttu-id="d0a49-256">Vstupní data v tomto kurzu jsou v souboru emp.txt, který není generován tímto kanálem, proto jsme tuto vlastnost nastavili na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="d0a49-256">The input data in this tutorial is in the emp.txt file, which is not generated by this pipeline, so we set this property to true.</span></span> |

    <span data-ttu-id="d0a49-257">Další informace o těchto vlastnostech JSON najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="d0a49-257">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="d0a49-258">Spuštěním následujícího příkazu vytvořte datovou sadu služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d0a49-258">Run the following command to create the Data Factory dataset.</span></span>

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    <span data-ttu-id="d0a49-259">Zde je ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="d0a49-259">Here is the sample output:</span></span>

    ```
    DatasetName       : InputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureBlobDataset
    Policy            : Microsoft.Azure.Management.DataFactories.Common.Models.Policy
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

### <a name="create-an-output-dataset"></a><span data-ttu-id="d0a49-260">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="d0a49-260">Create an output dataset</span></span>
<span data-ttu-id="d0a49-261">V této části kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-261">In this part of the step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="d0a49-262">Tato datová sada odkazuje na tabulku SQL ve službě Azure SQL Database, kterou reprezentuje **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-262">This dataset points to a SQL table in the Azure SQL database represented by **AzureSqlLinkedService**.</span></span> 

1. <span data-ttu-id="d0a49-263">Ve složce **C:\ADFGetStartedPSH** vytvořte soubor JSON s názvem **OutputDataset.json** s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d0a49-263">Create a JSON file named **OutputDataset.json** in the **C:\ADFGetStartedPSH** folder with the following content:</span></span>

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "structure": [
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

    <span data-ttu-id="d0a49-264">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="d0a49-264">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="d0a49-265">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="d0a49-265">Property</span></span> | <span data-ttu-id="d0a49-266">Popis</span><span class="sxs-lookup"><span data-stu-id="d0a49-266">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="d0a49-267">type</span><span class="sxs-lookup"><span data-stu-id="d0a49-267">type</span></span> | <span data-ttu-id="d0a49-268">Vlastnost type je nastavena na hodnotu **AzureSqlTable**, protože data se kopírují do tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="d0a49-268">The type property is set to **AzureSqlTable** because data is copied to a table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="d0a49-269">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="d0a49-269">linkedServiceName</span></span> | <span data-ttu-id="d0a49-270">Odkazuje na službu **AzureSqlLinkedService**, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="d0a49-270">Refers to the **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="d0a49-271">tableName</span><span class="sxs-lookup"><span data-stu-id="d0a49-271">tableName</span></span> | <span data-ttu-id="d0a49-272">Určuje **tabulku**, do které se kopírují data.</span><span class="sxs-lookup"><span data-stu-id="d0a49-272">Specified the **table** to which the data is copied.</span></span> | 
    | <span data-ttu-id="d0a49-273">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="d0a49-273">frequency/interval</span></span> | <span data-ttu-id="d0a49-274">Frekvence je nastavená na hodnotu **Hour** (hodina) a interval je **1**, což znamená, že výstupní řezy se tvoří **každou hodinu** mezi časy spuštění a ukončení, ne před nebo po této době.</span><span class="sxs-lookup"><span data-stu-id="d0a49-274">The frequency is set to **Hour** and interval is **1**, which means that the output slices are produced **hourly** between the pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="d0a49-275">V tabulce emp v databázi jsou k dispozici tři sloupce – **ID**, **FirstName** a **LastName**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-275">There are three columns – **ID**, **FirstName**, and **LastName** – in the emp table in the database.</span></span> <span data-ttu-id="d0a49-276">ID je sloupec identity, takže je zde třeba zadat pouze položky **FirstName** (Jméno) a **LastName** (Příjmení).</span><span class="sxs-lookup"><span data-stu-id="d0a49-276">ID is an identity column, so you need to specify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="d0a49-277">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="d0a49-277">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="d0a49-278">Spuštěním následujícího příkazu vytvořte datovou sadu datové továrny.</span><span class="sxs-lookup"><span data-stu-id="d0a49-278">Run the following command to create the data factory dataset.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    <span data-ttu-id="d0a49-279">Zde je ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="d0a49-279">Here is the sample output:</span></span>

    ```
    DatasetName       : OutputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureSqlTableDataset
    Policy            :
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

## <a name="create-a-pipeline"></a><span data-ttu-id="d0a49-280">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="d0a49-280">Create a pipeline</span></span>
<span data-ttu-id="d0a49-281">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="d0a49-281">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="d0a49-282">Výstupní datové sady v současné době řídí plán.</span><span class="sxs-lookup"><span data-stu-id="d0a49-282">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="d0a49-283">V tomto kurzu je výstupní datová sada nakonfigurovaná tak, aby vytvářela řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-283">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="d0a49-284">Kanál má čas spuštění a čas ukončení nastavený jeden den od sebe, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="d0a49-284">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="d0a49-285">Proto kanál vytvoří 24 řezů výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="d0a49-285">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span> 


1. <span data-ttu-id="d0a49-286">Ve složce **C:\ADFGetStartedPSH** vytvořte soubor JSON s názvem **ADFTutorialPipeline.json** s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="d0a49-286">Create a JSON file named **ADFTutorialPipeline.json** in the **C:\ADFGetStartedPSH** folder, with the following content:</span></span>

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```
    <span data-ttu-id="d0a49-287">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="d0a49-287">Note the following points:</span></span>
   
    - <span data-ttu-id="d0a49-288">V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **Copy**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-288">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="d0a49-289">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-289">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="d0a49-290">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="d0a49-291">Vstup aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-291">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span> 
    - <span data-ttu-id="d0a49-292">V části **typeProperties** je jako typ zdroje určen **BlobSource** a jako typ jímky **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-292">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="d0a49-293">Úplný seznam úložišť dat podporovaných aktivitou kopírování jako zdroje a jímky najdete v tématu [podporovaných úložištích dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="d0a49-293">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="d0a49-294">Kliknutím na odkaz v tabulce se dozvíte, jak použít konkrétní podporovaná úložiště dat jako zdroj/jímku.</span><span class="sxs-lookup"><span data-stu-id="d0a49-294">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>  
     
    <span data-ttu-id="d0a49-295">Nahraďte hodnotu vlastnosti **start** aktuálním dnem a **end** následujícím dnem.</span><span class="sxs-lookup"><span data-stu-id="d0a49-295">Replace the value of the **start** property with the current day and **end** value with the next day.</span></span> <span data-ttu-id="d0a49-296">Můžete zadat jenom část data a přeskočit část času.</span><span class="sxs-lookup"><span data-stu-id="d0a49-296">You can specify only the date part and skip the time part of the date time.</span></span> <span data-ttu-id="d0a49-297">Například „2016-02-03“ je ekvivalentní hodnotě „2016-02-03T00:00:00Z“.</span><span class="sxs-lookup"><span data-stu-id="d0a49-297">For example, "2016-02-03", which is equivalent to "2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="d0a49-298">Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="d0a49-298">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="d0a49-299">Například: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="d0a49-299">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="d0a49-300">Čas hodnoty **end** je nepovinný, ale my ho v tomto kurzu použijeme.</span><span class="sxs-lookup"><span data-stu-id="d0a49-300">The **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="d0a49-301">Pokud nezadáte hodnotu vlastnosti **end**, vypočítá se jako „**start + 48 hodin**“.</span><span class="sxs-lookup"><span data-stu-id="d0a49-301">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="d0a49-302">Pokud chcete kanál spouštět bez omezení, zadejte vlastnosti **end** hodnotu **9999-09-09**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-302">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span>
     
    <span data-ttu-id="d0a49-303">V předchozím příkladu je 24 datových řezů, protože se vytvářejí každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-303">In the preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="d0a49-304">Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-304">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="d0a49-305">Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-305">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="d0a49-306">Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-306">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="d0a49-307">Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="d0a49-307">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
2. <span data-ttu-id="d0a49-308">Spuštěním následujícího příkazu vytvořte tabulku datové továrny.</span><span class="sxs-lookup"><span data-stu-id="d0a49-308">Run the following command to create the data factory table.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    <span data-ttu-id="d0a49-309">Zde je ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="d0a49-309">Here is the sample output:</span></span> 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

<span data-ttu-id="d0a49-310">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="d0a49-310">**Congratulations!**</span></span> <span data-ttu-id="d0a49-311">Úspěšně jste vytvořili datovou továrnu Azure s kanálem, který kopíruje data z úložiště objektů blob v Azure do databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="d0a49-311">You have successfully created an Azure data factory with a pipeline to copy data from an Azure blob storage to an Azure SQL database.</span></span> 

## <a name="monitor-the-pipeline"></a><span data-ttu-id="d0a49-312">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="d0a49-312">Monitor the pipeline</span></span>
<span data-ttu-id="d0a49-313">V tomto kroku budete pomocí prostředí Azure PowerShell monitorovat, co se děje v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-313">In this step, you use Azure PowerShell to monitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="d0a49-314">Nahraďte &lt;DataFactoryName&gt; názvem své datové továrny, spusťte příkaz **Get-AzureRmDataFactory** a přiřaďte výstup k proměnné $df.</span><span class="sxs-lookup"><span data-stu-id="d0a49-314">Replace &lt;DataFactoryName&gt; with the name of your data factory and run **Get-AzureRmDataFactory**, and assign the output to a variable $df.</span></span>

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    <span data-ttu-id="d0a49-315">Například:</span><span class="sxs-lookup"><span data-stu-id="d0a49-315">For example:</span></span>
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    <span data-ttu-id="d0a49-316">Potom vytisknutím obsahu $df zobrazte následující výstup:</span><span class="sxs-lookup"><span data-stu-id="d0a49-316">Then, run print the contents of $df to see the following output:</span></span> 
    
    ```
    PS C:\ADFGetStartedPSH> $df
    
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DataFactoryId     : 6f194b34-03b3-49ab-8f03-9f8a7b9d3e30
    ResourceGroupName : ADFTutorialResourceGroup
    Location          : West US
    Tags              : {}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DataFactoryProperties
    ProvisioningState : Succeeded
    ```
2. <span data-ttu-id="d0a49-317">Spusťte rutinu **Get-AzureRmDataFactorySlice**, abyste získali podrobnosti o všech řezech datové sady **OutputDataset**, která je výstupní datovou sadou tohoto kanálu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-317">Run **Get-AzureRmDataFactorySlice** to get details about all slices of the **OutputDataset**, which is the output dataset of the pipeline.</span></span>  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   <span data-ttu-id="d0a49-318">Toto nastavení by mělo odpovídat hodnotě **Start** v kódu JSON kanálu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-318">This setting should match the **Start** value in the pipeline JSON.</span></span> <span data-ttu-id="d0a49-319">Měli byste vidět 24 řezů, pro každou hodinu od 12:00 aktuálního dne do 12:00 následujícího dne jeden.</span><span class="sxs-lookup"><span data-stu-id="d0a49-319">You should see 24 slices, one for each hour from 12 AM of the current day to 12 AM of the next day.</span></span>

   <span data-ttu-id="d0a49-320">Tady jsou tři ukázkové řezy z výstupu:</span><span class="sxs-lookup"><span data-stu-id="d0a49-320">Here are three sample slices from the output:</span></span> 

    ``` 
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 11:00:00 PM
    End               : 5/12/2017 12:00:00 AM
    RetryCount        : 0
    State             : Ready
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 9:00:00 PM
    End               : 5/11/2017 10:00:00 PM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0   

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 8:00:00 PM
    End               : 5/11/2017 9:00:00 PM
    RetryCount        : 0
    State             : Waiting
    SubState          : ConcurrencyLimit
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. <span data-ttu-id="d0a49-321">Spuštěním rutiny **Get-AzureRmDataFactoryRun** získáte podrobnosti o spouštění aktivity pro **konkrétní** řez.</span><span class="sxs-lookup"><span data-stu-id="d0a49-321">Run **Get-AzureRmDataFactoryRun** to get the details of activity runs for a **specific** slice.</span></span> <span data-ttu-id="d0a49-322">Zkopírujte hodnoty data a času z výstupu předchozího příkazu, abyste určili hodnotu parametru StartDateTime.</span><span class="sxs-lookup"><span data-stu-id="d0a49-322">Copy the date-time value from the output of the previous command to specify the value for the StartDateTime parameter.</span></span> 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   <span data-ttu-id="d0a49-323">Zde je ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="d0a49-323">Here is the sample output:</span></span> 

    ```
    Id                  : c0ddbd75-d0c7-4816-a775-704bbd7c7eab_636301332000000000_636301368000000000_OutputDataset
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH0516
    DatasetName         : OutputDataset
    ProcessingStartTime : 5/16/2017 8:00:33 PM
    ProcessingEndTime   : 5/16/2017 8:01:36 PM
    PercentComplete     : 100
    DataSliceStart      : 5/11/2017 9:00:00 PM
    DataSliceEnd        : 5/11/2017 10:00:00 PM
    Status              : Succeeded
    Timestamp           : 5/16/2017 8:00:33 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy  
    ```

<span data-ttu-id="d0a49-324">Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).</span><span class="sxs-lookup"><span data-stu-id="d0a49-324">For comprehensive documentation on Data Factory cmdlets, see [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories).</span></span>

## <a name="summary"></a><span data-ttu-id="d0a49-325">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d0a49-325">Summary</span></span>
<span data-ttu-id="d0a49-326">V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d0a49-326">In this tutorial, you created an Azure data factory to copy data from an Azure blob to an Azure SQL database.</span></span> <span data-ttu-id="d0a49-327">PowerShell jste použili k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="d0a49-327">You used PowerShell to create the data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="d0a49-328">Zde jsou základní kroky, které jste v tomto kurzu provedli:</span><span class="sxs-lookup"><span data-stu-id="d0a49-328">Here are the high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="d0a49-329">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="d0a49-329">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="d0a49-330">Vytvořili jste **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="d0a49-330">Created **linked services**:</span></span>

   <span data-ttu-id="d0a49-331">a.</span><span class="sxs-lookup"><span data-stu-id="d0a49-331">a.</span></span> <span data-ttu-id="d0a49-332">Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="d0a49-332">An **Azure Storage** linked service to link your Azure storage account that holds input data.</span></span>     
   <span data-ttu-id="d0a49-333">b.</span><span class="sxs-lookup"><span data-stu-id="d0a49-333">b.</span></span> <span data-ttu-id="d0a49-334">Propojená služba **Azure SQL** připojující službu SQL Database, která obsahuje výstupní data.</span><span class="sxs-lookup"><span data-stu-id="d0a49-334">An **Azure SQL** linked service to link your SQL database that holds the output data.</span></span>
3. <span data-ttu-id="d0a49-335">Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.</span><span class="sxs-lookup"><span data-stu-id="d0a49-335">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="d0a49-336">Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="d0a49-336">Created a **pipeline** with **Copy Activity**, with **BlobSource** as the source and **SqlSink** as the sink.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0a49-337">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0a49-337">Next steps</span></span>
<span data-ttu-id="d0a49-338">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="d0a49-338">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="d0a49-339">Následující tabulka obsahuje seznam úložišť dat podporovaných jako zdroje a cíle aktivitou kopírování:</span><span class="sxs-lookup"><span data-stu-id="d0a49-339">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="d0a49-340">Kliknutím na odkaz úložiště dat v tabulce získáte další informace o kopírování dat do nebo z úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="d0a49-340">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span> 

