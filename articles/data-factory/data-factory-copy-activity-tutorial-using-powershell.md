---
title: "Kurz: Vytvoření kanálu toomove dat pomocí Azure PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 21406d7dfaa0c555b2538fbb52839d761c140fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a><span data-ttu-id="6772d-103">Kurz: Vytvoření kanálu Data Factory pro přesouvání dat pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="6772d-103">Tutorial: Create a Data Factory pipeline that moves data by using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6772d-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="6772d-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="6772d-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="6772d-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="6772d-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6772d-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="6772d-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6772d-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="6772d-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6772d-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="6772d-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6772d-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="6772d-110">REST API</span><span class="sxs-lookup"><span data-stu-id="6772d-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="6772d-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="6772d-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

<span data-ttu-id="6772d-112">V tomto článku se dozvíte, jak toocreate toouse prostředí PowerShell objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-112">In this article, you learn how toouse PowerShell toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="6772d-113">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6772d-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="6772d-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="6772d-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="6772d-115">Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="6772d-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="6772d-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="6772d-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="6772d-117">Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="6772d-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="6772d-118">Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="6772d-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="6772d-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="6772d-120">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="6772d-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="6772d-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="6772d-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="6772d-122">Tento článek nepopisuje všechny rutiny služby Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-122">This article does not cover all hello Data Factory cmdlets.</span></span> <span data-ttu-id="6772d-123">Úplnou dokumentaci k těmto rutinám najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).</span><span class="sxs-lookup"><span data-stu-id="6772d-123">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on these cmdlets.</span></span>
> 
> <span data-ttu-id="6772d-124">Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="6772d-124">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="6772d-125">Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-125">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6772d-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6772d-126">Prerequisites</span></span>
- <span data-ttu-id="6772d-127">Dokončení požadavky uvedené v hello [požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6772d-127">Complete prerequisites listed in hello [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article.</span></span>
- <span data-ttu-id="6772d-128">Nainstalujte **Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6772d-128">Install **Azure PowerShell**.</span></span> <span data-ttu-id="6772d-129">Postupujte podle pokynů hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-129">Follow hello instructions in [How tooinstall and configure Azure PowerShell](../powershell-install-configure.md).</span></span>

## <a name="steps"></a><span data-ttu-id="6772d-130">Kroky</span><span class="sxs-lookup"><span data-stu-id="6772d-130">Steps</span></span>
<span data-ttu-id="6772d-131">Zde jsou hello kroky, které můžete provádět v rámci tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="6772d-131">Here are hello steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="6772d-132">Vytvořte **datovou továrnu** Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-132">Create an Azure **data factory**.</span></span> <span data-ttu-id="6772d-133">V tomto kroku vytvoříte datovou továrnu s názvem ADFTutorialDataFactoryPSH.</span><span class="sxs-lookup"><span data-stu-id="6772d-133">In this step, you create a data factory named ADFTutorialDataFactoryPSH.</span></span> 
2. <span data-ttu-id="6772d-134">Vytvoření **propojené služby** v datové továrně hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-134">Create **linked services** in hello data factory.</span></span> <span data-ttu-id="6772d-135">V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6772d-135">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="6772d-136">Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="6772d-136">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="6772d-137">Vytvořit kontejner a účet úložiště dat toothis odesílané jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-137">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="6772d-138">AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="6772d-138">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="6772d-139">Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi.</span><span class="sxs-lookup"><span data-stu-id="6772d-139">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="6772d-140">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.</span><span class="sxs-lookup"><span data-stu-id="6772d-140">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="6772d-141">Vytvoření vstupní a výstupní **datové sady** v datové továrně hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-141">Create input and output **datasets** in hello data factory.</span></span>  
    
    <span data-ttu-id="6772d-142">Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-142">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="6772d-143">A určuje datovou sadu vstupního objektu blob hello hello kontejneru a složce hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-143">And, hello input blob dataset specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="6772d-144">Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="6772d-144">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="6772d-145">A hello výstupní SQL tabulky datovou sadu Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-145">And, hello output SQL table dataset specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>
4. <span data-ttu-id="6772d-146">Vytvoření **kanálu** v datové továrně hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-146">Create a **pipeline** in hello data factory.</span></span> <span data-ttu-id="6772d-147">V tomto kroku pomocí aktivity kopírování vytvoříte kanál.</span><span class="sxs-lookup"><span data-stu-id="6772d-147">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="6772d-148">Aktivita kopírování Hello zkopíruje data z objektu blob ve tabulku tooa úložiště objektů blob v Azure hello ve hello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="6772d-148">hello copy activity copies data from a blob in hello Azure blob storage tooa table in hello Azure SQL database.</span></span> <span data-ttu-id="6772d-149">Můžete vytvořit aktivity kopírování v kanálu toocopy data z cílového všechny podporované zdrojové tooany podporována.</span><span class="sxs-lookup"><span data-stu-id="6772d-149">You can use a copy activity in a pipeline toocopy data from any supported source tooany supported destination.</span></span> <span data-ttu-id="6772d-150">Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="6772d-150">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="6772d-151">Monitorování kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-151">Monitor hello pipeline.</span></span> <span data-ttu-id="6772d-152">V tomto kroku budete **monitorování** hello řezy vstupní a výstupní datové sady pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6772d-152">In this step, you **monitor** hello slices of input and output datasets by using PowerShell.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="6772d-153">Vytvoření datové továrny</span><span class="sxs-lookup"><span data-stu-id="6772d-153">Create a data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6772d-154">Dokončení [předpoklady pro kurz hello](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="6772d-154">Complete [prerequisites for hello tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="6772d-155">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="6772d-155">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="6772d-156">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="6772d-156">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="6772d-157">Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data.</span><span class="sxs-lookup"><span data-stu-id="6772d-157">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="6772d-158">Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="6772d-158">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="6772d-159">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6772d-159">Launch **PowerShell**.</span></span> <span data-ttu-id="6772d-160">Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6772d-160">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="6772d-161">Pokud zavřete a znovu otevřete, toorun hello příkazů je třeba znovu.</span><span class="sxs-lookup"><span data-stu-id="6772d-161">If you close and reopen, you need toorun hello commands again.</span></span>

    <span data-ttu-id="6772d-162">Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="6772d-162">Run hello following command, and enter hello user name and password that you use toosign in toohello Azure portal:</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    <span data-ttu-id="6772d-163">Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet:</span><span class="sxs-lookup"><span data-stu-id="6772d-163">Run hello following command tooview all hello subscriptions for this account:</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="6772d-164">Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-164">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="6772d-165">Nahraďte  **&lt;NameOfAzureSubscription** &gt; s názvem hello předplatného Azure:</span><span class="sxs-lookup"><span data-stu-id="6772d-165">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription:</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. <span data-ttu-id="6772d-166">Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6772d-166">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    <span data-ttu-id="6772d-167">Některé hello kroky v tomto kurzu vychází z předpokladu, že používáte hello skupinu prostředků s názvem **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="6772d-167">Some of hello steps in this tutorial assume that you use hello resource group named **ADFTutorialResourceGroup**.</span></span> <span data-ttu-id="6772d-168">Pokud používáte jiné skupině prostředků, je nutné toouse ho místo skupiny ADFTutorialResourceGroup v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6772d-168">If you use a different resource group, you need toouse it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="6772d-169">Spustit hello **New-AzureRmDataFactory** rutiny toocreate objekt pro vytváření dat s názvem **ADFTutorialDataFactoryPSH**:</span><span class="sxs-lookup"><span data-stu-id="6772d-169">Run hello **New-AzureRmDataFactory** cmdlet toocreate a data factory named **ADFTutorialDataFactoryPSH**:</span></span>  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    <span data-ttu-id="6772d-170">Tento název už nejspíš není volný.</span><span class="sxs-lookup"><span data-stu-id="6772d-170">This name may already have been taken.</span></span> <span data-ttu-id="6772d-171">Proto zkontrolujte hello název objektu pro vytváření dat hello jedinečný přidáním předpony nebo přípony (například: ADFTutorialDataFactoryPSH05152017) a znovu spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-171">Therefore, make hello name of hello data factory unique by adding a prefix or suffix (for example: ADFTutorialDataFactoryPSH05152017) and run hello command again.</span></span>  

<span data-ttu-id="6772d-172">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="6772d-172">Note hello following points:</span></span>

* <span data-ttu-id="6772d-173">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6772d-173">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="6772d-174">Pokud se zobrazí následující chyba hello, změňte název hello (například na název Váš_název_adftutorialdatafactorypsh).</span><span class="sxs-lookup"><span data-stu-id="6772d-174">If you receive hello following error, change hello name (for example, yournameADFTutorialDataFactoryPSH).</span></span> <span data-ttu-id="6772d-175">Při provádění postupů v tomto kurzu potom tento název používejte místo názvu ADFTutorialFactoryPSH.</span><span class="sxs-lookup"><span data-stu-id="6772d-175">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="6772d-176">Informace o artefaktech služby Data Factory najdete v tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-176">See [Data Factory - Naming Rules](data-factory-naming-rules.md) for Data Factory artifacts.</span></span>

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* <span data-ttu-id="6772d-177">instance služby Data Factory toocreate, musí být Přispěvatel nebo správce hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-177">toocreate Data Factory instances, you must be a contributor or administrator of hello Azure subscription.</span></span>
* <span data-ttu-id="6772d-178">Hello název objektu pro vytváření dat hello může zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="6772d-178">hello name of hello data factory may be registered as a DNS name in hello future, and hence become publicly visible.</span></span>
* <span data-ttu-id="6772d-179">Můžete obdržet hello následující chybě: "**toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory.**"</span><span class="sxs-lookup"><span data-stu-id="6772d-179">You may receive hello following error: "**This subscription is not registered toouse namespace Microsoft.DataFactory.**"</span></span> <span data-ttu-id="6772d-180">Udělejte jednu z následujících hello a zkuste znovu publikovat:</span><span class="sxs-lookup"><span data-stu-id="6772d-180">Do one of hello following, and try publishing again:</span></span>

  * <span data-ttu-id="6772d-181">V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-181">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    <span data-ttu-id="6772d-182">Spusťte následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="6772d-182">Run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="6772d-183">Přihlášení pomocí hello předplatného Azure toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6772d-183">Sign in by using hello Azure subscription toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6772d-184">Okno objekt pro vytváření dat tooa přejít, nebo vytvořte objekt pro vytváření dat v hello portál Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-184">Go tooa Data Factory blade, or create a data factory in hello Azure portal.</span></span> <span data-ttu-id="6772d-185">Tato akce automaticky registruje zprostředkovatele hello za vás.</span><span class="sxs-lookup"><span data-stu-id="6772d-185">This action automatically registers hello provider for you.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="6772d-186">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="6772d-186">Create linked services</span></span>
<span data-ttu-id="6772d-187">Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="6772d-187">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="6772d-188">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="6772d-188">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="6772d-189">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="6772d-189">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="6772d-190">Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="6772d-190">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="6772d-191">Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="6772d-191">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="6772d-192">Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-192">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="6772d-193">AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="6772d-193">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="6772d-194">Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi.</span><span class="sxs-lookup"><span data-stu-id="6772d-194">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="6772d-195">Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-195">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="6772d-196">Vytvoření propojené služby pro účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6772d-196">Create a linked service for an Azure storage account</span></span>
<span data-ttu-id="6772d-197">V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-197">In this step, you link your Azure storage account tooyour data factory.</span></span>

1. <span data-ttu-id="6772d-198">Vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** v **C:\ADFGetStartedPSH** složku s hello následující obsah: (vytvořit hello složka ADFGetStartedPSH, pokud ještě neexistuje.)</span><span class="sxs-lookup"><span data-stu-id="6772d-198">Create a JSON file named **AzureStorageLinkedService.json** in **C:\ADFGetStartedPSH** folder with hello following content: (Create hello folder ADFGetStartedPSH if it does not already exist.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6772d-199">Nahraďte &lt;accountname&gt; a &lt;accountkey&gt; zadejte název a klíč účtu úložiště Azure před uložením souboru hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-199">Replace &lt;accountname&gt; and &lt;accountkey&gt; with name and key of your Azure storage account before saving hello file.</span></span> 

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
2. <span data-ttu-id="6772d-200">V **prostředí Azure PowerShell**, přepínač toohello **ADFGetStartedPSH** složky.</span><span class="sxs-lookup"><span data-stu-id="6772d-200">In **Azure PowerShell**, switch toohello **ADFGetStartedPSH** folder.</span></span>
4. <span data-ttu-id="6772d-201">Spustit hello **New-AzureRmDataFactoryLinkedService** rutiny toocreate hello propojené službě: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="6772d-201">Run hello **New-AzureRmDataFactoryLinkedService** cmdlet toocreate hello linked service: **AzureStorageLinkedService**.</span></span> <span data-ttu-id="6772d-202">Tato rutina a jiné rutiny služby Data Factory používané v tomto kurzu vyžaduje, aby vám toopass hodnoty pro hello **ResourceGroupName** a **DataFactoryName** parametry.</span><span class="sxs-lookup"><span data-stu-id="6772d-202">This cmdlet, and other Data Factory cmdlets you use in this tutorial requires you toopass values for hello **ResourceGroupName** and **DataFactoryName** parameters.</span></span> <span data-ttu-id="6772d-203">Alternativně můžete předat objekt DataFactory hello vrácený hello rutiny New-AzureRmDataFactory, aniž by museli zadávat ResourceGroupName a DataFactoryName pokaždé, když spustíte rutinu.</span><span class="sxs-lookup"><span data-stu-id="6772d-203">Alternatively, you can pass hello DataFactory object returned by hello New-AzureRmDataFactory cmdlet without typing ResourceGroupName and DataFactoryName each time you run a cmdlet.</span></span> 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    <span data-ttu-id="6772d-204">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-204">Here is hello sample output:</span></span>

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    <span data-ttu-id="6772d-205">Druhý způsob vytváření Tato propojená služba je název skupiny prostředků toospecify a název objektu pro vytváření dat místo zadání objekt DataFactory hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-205">Other way of creating this linked service is toospecify resource group name and data factory name instead of specifying hello DataFactory object.</span></span>  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a><span data-ttu-id="6772d-206">Vytvoření propojené služby pro Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="6772d-206">Create a linked service for an Azure SQL database</span></span>
<span data-ttu-id="6772d-207">V tomto kroku propojíte datovou továrnu tooyour databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="6772d-207">In this step, you link your Azure SQL database tooyour data factory.</span></span>

1. <span data-ttu-id="6772d-208">Vytvořte soubor JSON s názvem AzureSqlLinkedService.json ve složce C:\ADFGetStartedPSH s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="6772d-208">Create a JSON file named AzureSqlLinkedService.json in C:\ADFGetStartedPSH folder with hello following content:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6772d-209">Položky &lt;název_serveru&gt;, &lt;název_databáze&gt;, &lt;username@servername&gt; a &lt;heslo&gt; nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským účtem a heslem.</span><span class="sxs-lookup"><span data-stu-id="6772d-209">Replace &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt;, and &lt;password&gt; with names of your Azure SQL server, database, user account, and password.</span></span>
    
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
2. <span data-ttu-id="6772d-210">Spusťte následující příkaz toocreate hello propojené služby:</span><span class="sxs-lookup"><span data-stu-id="6772d-210">Run hello following command toocreate a linked service:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    <span data-ttu-id="6772d-211">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-211">Here is hello sample output:</span></span>

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   <span data-ttu-id="6772d-212">Potvrďte, že **povolit přístup k službám tooAzure** je zapnuté nastavení pro server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="6772d-212">Confirm that **Allow access tooAzure services** setting is turned on for your SQL database server.</span></span> <span data-ttu-id="6772d-213">tooverify a zapnout ho, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6772d-213">tooverify and turn it on, do hello following steps:</span></span>

    1. <span data-ttu-id="6772d-214">Přihlaste se toohello [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="6772d-214">Log in toohello [Azure portal](https://portal.azure.com)</span></span>
    2. <span data-ttu-id="6772d-215">Klikněte na tlačítko **další služby >** na levé straně hello a klikněte na **servery SQL** v hello **databáze** kategorie.</span><span class="sxs-lookup"><span data-stu-id="6772d-215">Click **More services >** on hello left, and click **SQL servers** in hello **DATABASES** category.</span></span>
    3. <span data-ttu-id="6772d-216">V seznamu hello systému SQL Server, vyberte svůj server.</span><span class="sxs-lookup"><span data-stu-id="6772d-216">Select your server in hello list of SQL servers.</span></span>
    4. <span data-ttu-id="6772d-217">V okně SQL serveru text hello, klikněte na tlačítko **zobrazit nastavení brány firewall** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6772d-217">On hello SQL server blade, click **Show firewall settings** link.</span></span>
    5. <span data-ttu-id="6772d-218">V hello **nastavení brány Firewall** okně klikněte na tlačítko **ON** pro **povolit přístup k službám tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="6772d-218">In hello **Firewall settings** blade, click **ON** for **Allow access tooAzure services**.</span></span>
    6. <span data-ttu-id="6772d-219">Klikněte na tlačítko **Uložit** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-219">Click **Save** on hello toolbar.</span></span> 

## <a name="create-datasets"></a><span data-ttu-id="6772d-220">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="6772d-220">Create datasets</span></span>
<span data-ttu-id="6772d-221">V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="6772d-221">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="6772d-222">V tomto kroku definujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService a AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="6772d-222">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="6772d-223">Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-223">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="6772d-224">A datovou sadu hello vstupního objektu blob (InputDataset) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-224">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="6772d-225">Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="6772d-225">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="6772d-226">A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-226">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-an-input-dataset"></a><span data-ttu-id="6772d-227">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="6772d-227">Create an input dataset</span></span>
<span data-ttu-id="6772d-228">V tomto kroku vytvoříte datové sady s názvem InputDataset, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService propojené služby.</span><span class="sxs-lookup"><span data-stu-id="6772d-228">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="6772d-229">Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="6772d-229">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="6772d-230">V tomto kurzu zadejte hodnotu pro název souboru hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-230">In this tutorial, you specify a value for hello fileName.</span></span>  

1. <span data-ttu-id="6772d-231">Vytvořte soubor JSON s názvem **InputDataset.json** v hello **C:\ADFGetStartedPSH** složku s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="6772d-231">Create a JSON file named **InputDataset.json** in hello **C:\ADFGetStartedPSH** folder, with hello following content:</span></span>

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

    <span data-ttu-id="6772d-232">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-232">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="6772d-233">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6772d-233">Property</span></span> | <span data-ttu-id="6772d-234">Popis</span><span class="sxs-lookup"><span data-stu-id="6772d-234">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="6772d-235">type</span><span class="sxs-lookup"><span data-stu-id="6772d-235">type</span></span> | <span data-ttu-id="6772d-236">Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="6772d-236">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="6772d-237">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="6772d-237">linkedServiceName</span></span> | <span data-ttu-id="6772d-238">Odkazuje toohello **AzureStorageLinkedService** kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="6772d-238">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="6772d-239">folderPath</span><span class="sxs-lookup"><span data-stu-id="6772d-239">folderPath</span></span> | <span data-ttu-id="6772d-240">Určuje objekt blob hello **kontejneru** a hello **složky** , který obsahuje vstupní objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="6772d-240">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="6772d-241">V tomto kurzu adftutorial hello kontejner objektů blob, složka hello kořenové složky.</span><span class="sxs-lookup"><span data-stu-id="6772d-241">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
    | <span data-ttu-id="6772d-242">fileName</span><span class="sxs-lookup"><span data-stu-id="6772d-242">fileName</span></span> | <span data-ttu-id="6772d-243">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="6772d-243">This property is optional.</span></span> <span data-ttu-id="6772d-244">Pokud ji vynecháte, jsou vybrány všechny soubory z hello folderPath.</span><span class="sxs-lookup"><span data-stu-id="6772d-244">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="6772d-245">V tomto kurzu **emp.txt** zadaný pro hello název souboru, takže pouze tento soubor je převzata pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="6772d-245">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="6772d-246">format -> type</span><span class="sxs-lookup"><span data-stu-id="6772d-246">format -> type</span></span> |<span data-ttu-id="6772d-247">vstupní soubor Hello je ve formátu textu hello, takže použijeme **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="6772d-247">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="6772d-248">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="6772d-248">columnDelimiter</span></span> | <span data-ttu-id="6772d-249">Hello sloupců ve vstupním souboru hello oddělené **čárku (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="6772d-249">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="6772d-250">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="6772d-250">frequency/interval</span></span> | <span data-ttu-id="6772d-251">je příliš nastavena frekvence Hello**hodinu** a interval je nastaven příliš**1**, což znamená, že hello vstupní řezy jsou dostupné **každou hodinu**.</span><span class="sxs-lookup"><span data-stu-id="6772d-251">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="6772d-252">Jinými slovy, hello služba Data Factory hledá vstupní data každou hodinu v kořenové složce hello kontejner objektů blob (**adftutorial**) jste zadali.</span><span class="sxs-lookup"><span data-stu-id="6772d-252">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="6772d-253">Hledá hello dat v rámci hello kanálu počáteční a koncové dobu, není před nebo po této doby.</span><span class="sxs-lookup"><span data-stu-id="6772d-253">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="6772d-254">external</span><span class="sxs-lookup"><span data-stu-id="6772d-254">external</span></span> | <span data-ttu-id="6772d-255">Tato vlastnost nastavena příliš**true** Pokud hello data nevygenerovala pomocí tohoto kanálu.</span><span class="sxs-lookup"><span data-stu-id="6772d-255">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="6772d-256">Hello vstupní data v tomto kurzu je v souboru emp.txt hello, který není generované tímto kanálem, abyste nám nastavit tuto vlastnost tootrue.</span><span class="sxs-lookup"><span data-stu-id="6772d-256">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

    <span data-ttu-id="6772d-257">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6772d-257">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="6772d-258">Spusťte následující příkaz datovou sadu služby Data Factory hello toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-258">Run hello following command toocreate hello Data Factory dataset.</span></span>

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    <span data-ttu-id="6772d-259">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-259">Here is hello sample output:</span></span>

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

### <a name="create-an-output-dataset"></a><span data-ttu-id="6772d-260">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="6772d-260">Create an output dataset</span></span>
<span data-ttu-id="6772d-261">V této části hello kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="6772d-261">In this part of hello step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="6772d-262">Tato datová sada body tooa tabulky SQL ve hello Azure SQL database reprezentované **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="6772d-262">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService**.</span></span> 

1. <span data-ttu-id="6772d-263">Vytvořte soubor JSON s názvem **OutputDataset.json** v hello **C:\ADFGetStartedPSH** složku s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="6772d-263">Create a JSON file named **OutputDataset.json** in hello **C:\ADFGetStartedPSH** folder with hello following content:</span></span>

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

    <span data-ttu-id="6772d-264">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-264">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="6772d-265">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6772d-265">Property</span></span> | <span data-ttu-id="6772d-266">Popis</span><span class="sxs-lookup"><span data-stu-id="6772d-266">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="6772d-267">type</span><span class="sxs-lookup"><span data-stu-id="6772d-267">type</span></span> | <span data-ttu-id="6772d-268">Hello vlastnost Typ nastavena příliš**AzureSqlTable** protože data jsou zkopírované tooa tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="6772d-268">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="6772d-269">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="6772d-269">linkedServiceName</span></span> | <span data-ttu-id="6772d-270">Odkazuje toohello **AzureSqlLinkedService** kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="6772d-270">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="6772d-271">tableName</span><span class="sxs-lookup"><span data-stu-id="6772d-271">tableName</span></span> | <span data-ttu-id="6772d-272">Zadaný hello **tabulky** toowhich hello data budou zkopírována.</span><span class="sxs-lookup"><span data-stu-id="6772d-272">Specified hello **table** toowhich hello data is copied.</span></span> | 
    | <span data-ttu-id="6772d-273">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="6772d-273">frequency/interval</span></span> | <span data-ttu-id="6772d-274">Hello je nastavena frekvence příliš**hodinu** a interval je **1**, což znamená, že vytváří řezy výstup hello **každou hodinu** mezi hello kanálu počáteční a koncové krát, ne dřív nebo Po této doby.</span><span class="sxs-lookup"><span data-stu-id="6772d-274">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="6772d-275">Existují tři sloupce – **ID**, **FirstName**, a **LastName** – v tabulce emp hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-275">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="6772d-276">ID je sloupec identity, takže je nutné pouze toospecify **FirstName** a **LastName** sem.</span><span class="sxs-lookup"><span data-stu-id="6772d-276">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="6772d-277">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6772d-277">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="6772d-278">Spusťte následující příkaz toocreate hello data factory dataset hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-278">Run hello following command toocreate hello data factory dataset.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    <span data-ttu-id="6772d-279">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-279">Here is hello sample output:</span></span>

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

## <a name="create-a-pipeline"></a><span data-ttu-id="6772d-280">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="6772d-280">Create a pipeline</span></span>
<span data-ttu-id="6772d-281">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="6772d-281">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="6772d-282">Výstupní datové sady v současné době je, jaké jednotky hello plán.</span><span class="sxs-lookup"><span data-stu-id="6772d-282">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="6772d-283">V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="6772d-283">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="6772d-284">Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="6772d-284">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="6772d-285">Proto 24 řezů výstupní datové sady vyprodukované hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="6772d-285">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 


1. <span data-ttu-id="6772d-286">Vytvořte soubor JSON s názvem **C:\adfgetstartedpsh** v hello **C:\ADFGetStartedPSH** složku s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="6772d-286">Create a JSON file named **ADFTutorialPipeline.json** in hello **C:\ADFGetStartedPSH** folder, with hello following content:</span></span>

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob tooAzure SQL table",
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
    <span data-ttu-id="6772d-287">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="6772d-287">Note hello following points:</span></span>
   
    - <span data-ttu-id="6772d-288">V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**.</span><span class="sxs-lookup"><span data-stu-id="6772d-288">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="6772d-289">Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-289">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="6772d-290">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="6772d-291">Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="6772d-291">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="6772d-292">V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-292">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="6772d-293">Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="6772d-293">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="6772d-294">toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-294">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
     
    <span data-ttu-id="6772d-295">Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den.</span><span class="sxs-lookup"><span data-stu-id="6772d-295">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="6772d-296">Můžete zadat jenom část data hello a přeskočit část času hello hello datum a čas.</span><span class="sxs-lookup"><span data-stu-id="6772d-296">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="6772d-297">Například "2016-02-03", který je ekvivalentní příliš "2016-02-03T00:00:00Z"</span><span class="sxs-lookup"><span data-stu-id="6772d-297">For example, "2016-02-03", which is equivalent too"2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="6772d-298">Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="6772d-298">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="6772d-299">Například: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="6772d-299">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="6772d-300">Hello **end** čas je nepovinný, ale používáme v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6772d-300">hello **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="6772d-301">Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**".</span><span class="sxs-lookup"><span data-stu-id="6772d-301">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="6772d-302">bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6772d-302">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
     
    <span data-ttu-id="6772d-303">V předchozím příkladu hello jsou 24 datových řezů jako se vytvářejí každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="6772d-303">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="6772d-304">Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-304">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="6772d-305">Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-305">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="6772d-306">Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-306">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="6772d-307">Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="6772d-307">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
2. <span data-ttu-id="6772d-308">Spusťte následující příkaz toocreate hello data factory tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-308">Run hello following command toocreate hello data factory table.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    <span data-ttu-id="6772d-309">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-309">Here is hello sample output:</span></span> 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

<span data-ttu-id="6772d-310">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="6772d-310">**Congratulations!**</span></span> <span data-ttu-id="6772d-311">Úspěšně jste vytvořili objekt pro vytváření dat Azure s kanálu toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-311">You have successfully created an Azure data factory with a pipeline toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> 

## <a name="monitor-hello-pipeline"></a><span data-ttu-id="6772d-312">Monitorování kanálu hello</span><span class="sxs-lookup"><span data-stu-id="6772d-312">Monitor hello pipeline</span></span>
<span data-ttu-id="6772d-313">V tomto kroku budete pomocí prostředí Azure PowerShell toomonitor co se děje v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-313">In this step, you use Azure PowerShell toomonitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="6772d-314">Nahraďte &lt;DataFactoryName&gt; hello název objektu pro vytváření dat a spusťte **Get-AzureRmDataFactory**a přiřaďte výstup tooa hello proměnné $df.</span><span class="sxs-lookup"><span data-stu-id="6772d-314">Replace &lt;DataFactoryName&gt; with hello name of your data factory and run **Get-AzureRmDataFactory**, and assign hello output tooa variable $df.</span></span>

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    <span data-ttu-id="6772d-315">Například:</span><span class="sxs-lookup"><span data-stu-id="6772d-315">For example:</span></span>
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    <span data-ttu-id="6772d-316">Potom spusťte tiskové hello obsah hello toosee $df následující výstup:</span><span class="sxs-lookup"><span data-stu-id="6772d-316">Then, run print hello contents of $df toosee hello following output:</span></span> 
    
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
2. <span data-ttu-id="6772d-317">Spustit **Get-AzureRmDataFactorySlice** tooget podrobnosti o všech řezech tabulky hello **OutputDataset**, což je hello výstupní datovou sadu hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="6772d-317">Run **Get-AzureRmDataFactorySlice** tooget details about all slices of hello **OutputDataset**, which is hello output dataset of hello pipeline.</span></span>  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   <span data-ttu-id="6772d-318">Toto nastavení by měl odpovídat hello **spustit** hodnota v kódu JSON kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-318">This setting should match hello **Start** value in hello pipeline JSON.</span></span> <span data-ttu-id="6772d-319">Měli byste vidět 24 řezů, pro každou hodinu od 12: 00 aktuálního dne too12 hello mě hello další den.</span><span class="sxs-lookup"><span data-stu-id="6772d-319">You should see 24 slices, one for each hour from 12 AM of hello current day too12 AM of hello next day.</span></span>

   <span data-ttu-id="6772d-320">Tady jsou tři ukázkové řezy z výstupu hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-320">Here are three sample slices from hello output:</span></span> 

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
3. <span data-ttu-id="6772d-321">Spustit **Get-AzureRmDataFactoryRun** tooget hello podrobnosti aktivity běží **konkrétní** řez.</span><span class="sxs-lookup"><span data-stu-id="6772d-321">Run **Get-AzureRmDataFactoryRun** tooget hello details of activity runs for a **specific** slice.</span></span> <span data-ttu-id="6772d-322">Zkopírujte hodnotu data a času hello z hello výstup hello předchozí příkaz toospecify hello hodnotu pro parametr StartDateTime hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-322">Copy hello date-time value from hello output of hello previous command toospecify hello value for hello StartDateTime parameter.</span></span> 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   <span data-ttu-id="6772d-323">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-323">Here is hello sample output:</span></span> 

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

<span data-ttu-id="6772d-324">Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).</span><span class="sxs-lookup"><span data-stu-id="6772d-324">For comprehensive documentation on Data Factory cmdlets, see [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories).</span></span>

## <a name="summary"></a><span data-ttu-id="6772d-325">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6772d-325">Summary</span></span>
<span data-ttu-id="6772d-326">V tomto kurzu jste vytvořili Azure data factory toocopy dat z Azure SQL database tooan objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-326">In this tutorial, you created an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="6772d-327">Můžete použít PowerShell toocreate hello objekt pro vytváření dat, propojených služeb, datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="6772d-327">You used PowerShell toocreate hello data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="6772d-328">Zde jsou hello kroků, které jste provedli v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="6772d-328">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="6772d-329">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="6772d-329">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="6772d-330">Vytvořili jste **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="6772d-330">Created **linked services**:</span></span>

   <span data-ttu-id="6772d-331">a.</span><span class="sxs-lookup"><span data-stu-id="6772d-331">a.</span></span> <span data-ttu-id="6772d-332">**Azure Storage** propojené služby toolink účtu úložiště Azure, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="6772d-332">An **Azure Storage** linked service toolink your Azure storage account that holds input data.</span></span>     
   <span data-ttu-id="6772d-333">b.</span><span class="sxs-lookup"><span data-stu-id="6772d-333">b.</span></span> <span data-ttu-id="6772d-334">**Azure SQL** propojené služby toolink vaší databázi SQL, který obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="6772d-334">An **Azure SQL** linked service toolink your SQL database that holds hello output data.</span></span>
3. <span data-ttu-id="6772d-335">Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.</span><span class="sxs-lookup"><span data-stu-id="6772d-335">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="6772d-336">Vytvoření **kanálu** s **aktivity kopírování**, s **BlobSource** jako zdroj hello a **SqlSink** jako jímku hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-336">Created a **pipeline** with **Copy Activity**, with **BlobSource** as hello source and **SqlSink** as hello sink.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6772d-337">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6772d-337">Next steps</span></span>
<span data-ttu-id="6772d-338">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="6772d-338">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="6772d-339">Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello:</span><span class="sxs-lookup"><span data-stu-id="6772d-339">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="6772d-340">toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="6772d-340">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span> 

