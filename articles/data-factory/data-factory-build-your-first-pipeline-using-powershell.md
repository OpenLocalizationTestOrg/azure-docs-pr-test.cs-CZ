---
title: "aaaBuild vaše první objekt pro vytváření dat (PowerShell) | Microsoft Docs"
description: "V tomto kurzu vytvoříte pomocí prostředí Azure PowerShell ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 22ec1236-ea86-4eb7-b903-0e79a58b90c7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 626260798b56d590577b3c4b24f7cf52873c9f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a><span data-ttu-id="ac7a8-103">Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac7a8-103">Tutorial: Build your first Azure data factory using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ac7a8-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="ac7a8-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="ac7a8-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ac7a8-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="ac7a8-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac7a8-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="ac7a8-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac7a8-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="ac7a8-108">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ac7a8-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="ac7a8-109">REST API</span><span class="sxs-lookup"><span data-stu-id="ac7a8-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

<span data-ttu-id="ac7a8-110">V tomto článku používáte prostředí Azure PowerShell toocreate první objekt pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-110">In this article, you use Azure PowerShell toocreate your first Azure data factory.</span></span> <span data-ttu-id="ac7a8-111">kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="ac7a8-112">Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="ac7a8-113">Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="ac7a8-114">Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="ac7a8-115">Hello datovém kanálu v tomto kurzu transformuje vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="ac7a8-116">Nekopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-116">It does not copy data from a source data store tooa destination data store.</span></span> <span data-ttu-id="ac7a8-117">Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-117">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="ac7a8-118">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-118">A pipeline can have more than one activity.</span></span> <span data-ttu-id="ac7a8-119">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-119">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="ac7a8-120">Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-120">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac7a8-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ac7a8-121">Prerequisites</span></span>
* <span data-ttu-id="ac7a8-122">Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-122">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="ac7a8-123">Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku tooinstall nejnovější verzi prostředí Azure PowerShell ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-123">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span>
* <span data-ttu-id="ac7a8-124">(volitelné) Tento článek nepopisuje všechny rutiny služby Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-124">(optional) This article does not cover all hello Data Factory cmdlets.</span></span> <span data-ttu-id="ac7a8-125">Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-125">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on Data Factory cmdlets.</span></span>

## <a name="create-data-factory"></a><span data-ttu-id="ac7a8-126">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="ac7a8-126">Create data factory</span></span>
<span data-ttu-id="ac7a8-127">V tomto kroku budete pomocí prostředí Azure PowerShell toocreate Azure Data Factory s názvem **FirstDataFactoryPSH**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-127">In this step, you use Azure PowerShell toocreate an Azure Data Factory named **FirstDataFactoryPSH**.</span></span> <span data-ttu-id="ac7a8-128">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-128">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="ac7a8-129">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-129">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="ac7a8-130">Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-130">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data.</span></span> <span data-ttu-id="ac7a8-131">Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-131">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="ac7a8-132">Otevřete prostředí Azure PowerShell a spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-132">Start Azure PowerShell and run hello following command.</span></span> <span data-ttu-id="ac7a8-133">Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-133">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="ac7a8-134">Pokud zavřete a znovu, musíte toorun tyto příkazy znovu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-134">If you close and reopen, you need toorun these commands again.</span></span>
   * <span data-ttu-id="ac7a8-135">Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-135">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
    ```PowerShell
    Login-AzureRmAccount
    ```    
   * <span data-ttu-id="ac7a8-136">Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-136">Run hello following command tooview all hello subscriptions for this account.</span></span>
    ```PowerShell
    Get-AzureRmSubscription 
    ```
   * <span data-ttu-id="ac7a8-137">Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-137">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="ac7a8-138">Toto předplatné by měl být hello stejné jako hello jeden, které jste použili v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-138">This subscription should be hello same as hello one you used in hello Azure portal.</span></span>
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```     
2. <span data-ttu-id="ac7a8-139">Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-139">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command:</span></span>
    
    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    <span data-ttu-id="ac7a8-140">Některé hello kroky v tomto kurzu vychází z předpokladu, že používáte hello skupinu prostředků s názvem ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-140">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="ac7a8-141">Pokud používáte jiné skupině prostředků, je nutné toouse ho místo skupiny ADFTutorialResourceGroup v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-141">If you use a different resource group, you need toouse it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="ac7a8-142">Spustit hello **New-AzureRmDataFactory** rutinu, která vytvoří objekt pro vytváření dat s názvem **FirstDataFactoryPSH**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-142">Run hello **New-AzureRmDataFactory** cmdlet that creates a data factory named **FirstDataFactoryPSH**.</span></span>

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"
    ```
<span data-ttu-id="ac7a8-143">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-143">Note hello following points:</span></span>

* <span data-ttu-id="ac7a8-144">Název Hello hello objekt pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-144">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="ac7a8-145">Pokud se zobrazí chyba hello **název objektu pro vytváření dat "FirstDataFactoryPSH" není k dispozici**, změňte název hello (například na název Váš_název_firstdatafactorypsh).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-145">If you receive hello error **Data factory name “FirstDataFactoryPSH” is not available**, change hello name (for example, yournameFirstDataFactoryPSH).</span></span> <span data-ttu-id="ac7a8-146">Při provádění postupů v tomto kurzu potom tento název používejte místo názvu ADFTutorialFactoryPSH.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-146">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="ac7a8-147">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-147">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
* <span data-ttu-id="ac7a8-148">instance služby Data Factory toocreate, je nutné toobe přispěvatelem/správcem předplatného Azure hello</span><span class="sxs-lookup"><span data-stu-id="ac7a8-148">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="ac7a8-149">Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-149">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>
* <span data-ttu-id="ac7a8-150">Pokud se zobrazí chyba hello: "**toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory**", proveďte jednu z následujících hello a zkuste to znovu publikovat:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-150">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span>

  * <span data-ttu-id="ac7a8-151">V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-151">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
      <span data-ttu-id="ac7a8-152">Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-152">You can run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="ac7a8-153">Přihlášení pomocí hello předplatného Azure hello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-153">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="ac7a8-154">Tato akce automaticky registruje zprostředkovatele hello za vás.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-154">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="ac7a8-155">Před vytvořením kanálu, je nutné toocreate několik entit služby Data Factory nejdřív.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-155">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="ac7a8-156">Nejdřív vytvoříte propojené služby toolink dat úložiště nebo výpočtů tooyour data ukládat, definujete vstupní a výstupní datové sady toorepresent vstupní a výstupní data v propojených úložištích dat a poté vytvořit hello kanálu s aktivitou, která používá tyto datové sady.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-156">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent input/output data in linked data stores, and then create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="ac7a8-157">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="ac7a8-157">Create linked services</span></span>
<span data-ttu-id="ac7a8-158">V tomto kroku propojíte účtu úložiště Azure a služby na vyžádání Azure HDInsight clusteru tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-158">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="ac7a8-159">blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-159">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="ac7a8-160">Hello propojené služby HDInsight je použité toorun skriptu Hive určeného v hello aktivitu hello kanál v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-160">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span> <span data-ttu-id="ac7a8-161">Určit data, která úložiště/výpočetní služby se používají ve vašem scénáři a vytvořením propojených služeb spojit tyto služby toohello objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-161">Identify what data store/compute services are used in your scenario and link those services toohello data factory by creating linked services.</span></span>

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="ac7a8-162">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="ac7a8-162">Create Azure Storage linked service</span></span>
<span data-ttu-id="ac7a8-163">V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-163">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="ac7a8-164">Hello použijete stejný účet úložiště Azure toostore vstupní a výstupní data a hello HQL soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-164">You use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="ac7a8-165">Vytvořte soubor JSON s názvem StorageLinkedService.json ve složce C:\ADFGetStarted hello s hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-165">Create a JSON file named StorageLinkedService.json in hello C:\ADFGetStarted folder with hello following content.</span></span> <span data-ttu-id="ac7a8-166">Pokud ještě neexistuje, vytvořte hello složky ADFGetStarted.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-166">Create hello folder ADFGetStarted if it does not already exist.</span></span>

    ```json
    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }
    ```
    <span data-ttu-id="ac7a8-167">Nahraďte **název účtu** hello název účtu úložiště Azure a **klíč účtu** s přístupovým klíčem hello hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-167">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="ac7a8-168">toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-168">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
2. <span data-ttu-id="ac7a8-169">V prostředí Azure PowerShell přepínače toohello složky ADFGetStarted.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-169">In Azure PowerShell, switch toohello ADFGetStarted folder.</span></span>
3. <span data-ttu-id="ac7a8-170">Můžete použít hello **New-AzureRmDataFactoryLinkedService** rutinu, která vytvoří propojené služby.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-170">You can use hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates a linked service.</span></span> <span data-ttu-id="ac7a8-171">Tato rutina a jiné rutiny služby Data Factory používané v tomto kurzu vyžaduje, aby vám toopass hodnoty pro hello *ResourceGroupName* a *DataFactoryName* parametry.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-171">This cmdlet and other Data Factory cmdlets you use in this tutorial requires you toopass values for hello *ResourceGroupName* and *DataFactoryName* parameters.</span></span> <span data-ttu-id="ac7a8-172">Alternativně můžete použít **Get-AzureRmDataFactory** tooget **DataFactory** objektu a hello objekt předat, aniž by museli zadávat *ResourceGroupName* a * DataFactoryName* pokaždé, když spustíte rutinu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-172">Alternatively, you can use **Get-AzureRmDataFactory** tooget a **DataFactory** object and pass hello object without typing *ResourceGroupName* and *DataFactoryName* each time you run a cmdlet.</span></span> <span data-ttu-id="ac7a8-173">Hello spusťte následující příkaz tooassign hello výstup hello **Get-AzureRmDataFactory** rutiny tooa **$df** proměnné.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-173">Run hello following command tooassign hello output of hello **Get-AzureRmDataFactory** cmdlet tooa **$df** variable.</span></span>

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
4. <span data-ttu-id="ac7a8-174">Nyní, spustit hello **New-AzureRmDataFactoryLinkedService** rutinu, která vytvoří hello propojené **StorageLinkedService** služby.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-174">Now, run hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates hello linked **StorageLinkedService** service.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```
    <span data-ttu-id="ac7a8-175">Pokud jste nespustili hello **Get-AzureRmDataFactory** rutiny a přiřazené hello výstupní toohello **$df** proměnné, byste měli toospecify hodnoty pro hello *ResourceGroupName*a *DataFactoryName* parametry následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-175">If you hadn't run hello **Get-AzureRmDataFactory** cmdlet and assigned hello output toohello **$df** variable, you would have toospecify values for hello *ResourceGroupName* and *DataFactoryName* parameters as follows.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json
    ```
    <span data-ttu-id="ac7a8-176">Pokud uprostřed hello hello kurzu zavřete prostředí Azure PowerShell, máte toorun hello **Get-AzureRmDataFactory** rutiny při příštím spuštění prostředí Azure PowerShell toocomplete hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-176">If you close Azure PowerShell in hello middle of hello tutorial, you have toorun hello **Get-AzureRmDataFactory** cmdlet next time you start Azure PowerShell toocomplete hello tutorial.</span></span>

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="ac7a8-177">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ac7a8-177">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="ac7a8-178">V tomto kroku propojíte služby na vyžádání HDInsight clusteru tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-178">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="ac7a8-179">Hello HDInsight cluster je automaticky vytvoří za běhu a až dokončí zpracování a nečinnosti hello zadaného množství času se odstraní.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-179">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span> <span data-ttu-id="ac7a8-180">Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-180">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="ac7a8-181">Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-181">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>

1. <span data-ttu-id="ac7a8-182">Vytvořte soubor JSON s názvem **HDInsightOnDemandLinkedService**.json v hello **C:\ADFGetStarted** složku s hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-182">Create a JSON file named **HDInsightOnDemandLinkedService**.json in hello **C:\ADFGetStarted** folder with hello following content.</span></span>

    ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }
    ```
    <span data-ttu-id="ac7a8-183">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-183">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="ac7a8-184">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ac7a8-184">Property</span></span> | <span data-ttu-id="ac7a8-185">Popis</span><span class="sxs-lookup"><span data-stu-id="ac7a8-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="ac7a8-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="ac7a8-186">ClusterSize</span></span> |<span data-ttu-id="ac7a8-187">Určuje velikost hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-187">Specifies hello size of hello HDInsight cluster.</span></span> |
   | <span data-ttu-id="ac7a8-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="ac7a8-188">TimeToLive</span></span> |<span data-ttu-id="ac7a8-189">Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-189">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="ac7a8-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="ac7a8-190">linkedServiceName</span></span> |<span data-ttu-id="ac7a8-191">Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem HDInsight</span><span class="sxs-lookup"><span data-stu-id="ac7a8-191">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight</span></span> |

    <span data-ttu-id="ac7a8-192">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-192">Note hello following points:</span></span>

   * <span data-ttu-id="ac7a8-193">Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello JSON.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-193">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello JSON.</span></span> <span data-ttu-id="ac7a8-194">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="ac7a8-195">Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="ac7a8-196">Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="ac7a8-197">Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-197">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="ac7a8-198">HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-198">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="ac7a8-199">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-199">This behavior is by design.</span></span> <span data-ttu-id="ac7a8-200">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je zpracován určitý řez, pokud neexistuje aktivní cluster (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="ac7a8-201">Hello clusteru je automaticky odstraněna po dokončení zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-201">hello cluster is automatically deleted when hello processing is done.</span></span>

       <span data-ttu-id="ac7a8-202">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="ac7a8-203">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-203">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="ac7a8-204">vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času".</span><span class="sxs-lookup"><span data-stu-id="ac7a8-204">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="ac7a8-205">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="ac7a8-206">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
2. <span data-ttu-id="ac7a8-207">Spustit hello **New-AzureRmDataFactoryLinkedService** rutinu, která vytvoří hello propojená služba s názvem HDInsightOnDemandLinkedService.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-207">Run hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates hello linked service called HDInsightOnDemandLinkedService.</span></span>
    
    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json
    ```

## <a name="create-datasets"></a><span data-ttu-id="ac7a8-208">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="ac7a8-208">Create datasets</span></span>
<span data-ttu-id="ac7a8-209">V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-209">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="ac7a8-210">Tyto datové sady odkazují toohello **StorageLinkedService** jste vytvořili dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-210">These datasets refer toohello **StorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="ac7a8-211">Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-211">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>

### <a name="create-input-dataset"></a><span data-ttu-id="ac7a8-212">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="ac7a8-212">Create input dataset</span></span>
1. <span data-ttu-id="ac7a8-213">Vytvořte soubor JSON s názvem **C:\adfgetstarted** v hello **C:\ADFGetStarted** složku s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-213">Create a JSON file named **InputTable.json** in hello **C:\ADFGetStarted** folder with hello following content:</span></span>

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
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
    <span data-ttu-id="ac7a8-214">Hello JSON Určuje datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-214">hello JSON defines a dataset named **AzureBlobInput**, which represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="ac7a8-215">Kromě toho určuje, zda text hello vstupní data nachází v kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-215">In addition, it specifies that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

    <span data-ttu-id="ac7a8-216">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-216">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="ac7a8-217">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ac7a8-217">Property</span></span> | <span data-ttu-id="ac7a8-218">Popis</span><span class="sxs-lookup"><span data-stu-id="ac7a8-218">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="ac7a8-219">type</span><span class="sxs-lookup"><span data-stu-id="ac7a8-219">type</span></span> |<span data-ttu-id="ac7a8-220">Hello vlastnost Typ nastavena tooAzureBlob, protože data se nachází v úložišti objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-220">hello type property is set tooAzureBlob because data resides in Azure blob storage.</span></span> |
   | <span data-ttu-id="ac7a8-221">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="ac7a8-221">linkedServiceName</span></span> |<span data-ttu-id="ac7a8-222">odkazuje toohello StorageLinkedService, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-222">refers toohello StorageLinkedService you created earlier.</span></span> |
   | <span data-ttu-id="ac7a8-223">fileName</span><span class="sxs-lookup"><span data-stu-id="ac7a8-223">fileName</span></span> |<span data-ttu-id="ac7a8-224">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-224">This property is optional.</span></span> <span data-ttu-id="ac7a8-225">Pokud ji vynecháte, jsou vybrány všechny soubory hello z hello folderPath.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-225">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="ac7a8-226">V takovém případě pouze hello soubor Input.log.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-226">In this case, only hello input.log is processed.</span></span> |
   | <span data-ttu-id="ac7a8-227">type</span><span class="sxs-lookup"><span data-stu-id="ac7a8-227">type</span></span> |<span data-ttu-id="ac7a8-228">soubory protokolu Hello jsou v textovém formátu, takže používáme TextFormat.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-228">hello log files are in text format, so we use TextFormat.</span></span> |
   | <span data-ttu-id="ac7a8-229">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="ac7a8-229">columnDelimiter</span></span> |<span data-ttu-id="ac7a8-230">sloupce v souborech protokolů hello jsou odděleny hello čárku (,).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-230">columns in hello log files are delimited by hello comma character (,).</span></span> |
   | <span data-ttu-id="ac7a8-231">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="ac7a8-231">frequency/interval</span></span> |<span data-ttu-id="ac7a8-232">frekvence nastavená tooMonth a interval je 1, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-232">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span> |
   | <span data-ttu-id="ac7a8-233">external</span><span class="sxs-lookup"><span data-stu-id="ac7a8-233">external</span></span> |<span data-ttu-id="ac7a8-234">Tato vlastnost nastavena tootrue, pokud hello vstupní data nevygenerovala služba Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-234">this property is set tootrue if hello input data is not generated by hello Data Factory service.</span></span> |
2. <span data-ttu-id="ac7a8-235">Spusťte následující příkaz v prostředí Azure PowerShell datovou sadu služby Data Factory hello toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-235">Run hello following command in Azure PowerShell toocreate hello Data Factory dataset:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\InputTable.json
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="ac7a8-236">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="ac7a8-236">Create output dataset</span></span>
<span data-ttu-id="ac7a8-237">Teď vytvořte hello výstupní datovou sadu toorepresent hello výstupní data ve hello úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-237">Now, you create hello output dataset toorepresent hello output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="ac7a8-238">Vytvořte soubor JSON s názvem **C:\adfgetstarted** v hello **C:\ADFGetStarted** složku s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-238">Create a JSON file named **OutputTable.json** in hello **C:\ADFGetStarted** folder with hello following content:</span></span>

    ```json
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    <span data-ttu-id="ac7a8-239">Hello JSON Určuje datovou sadu s názvem **AzureBlobOutput**, která představuje výstupní data pro aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-239">hello JSON defines a dataset named **AzureBlobOutput**, which represents output data for an activity in hello pipeline.</span></span> <span data-ttu-id="ac7a8-240">Kromě toho určuje, že hello výsledky ukládat do kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-240">In addition, it specifies that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="ac7a8-241">Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-241">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>
2. <span data-ttu-id="ac7a8-242">Spusťte následující příkaz v prostředí Azure PowerShell datovou sadu služby Data Factory hello toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-242">Run hello following command in Azure PowerShell toocreate hello Data Factory dataset:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\OutputTable.json
    ```

## <a name="create-pipeline"></a><span data-ttu-id="ac7a8-243">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="ac7a8-243">Create pipeline</span></span>
<span data-ttu-id="ac7a8-244">V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-244">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="ac7a8-245">Vstupní řez je dostupný jednou měsíčně (frekvence: Month, interval: 1), výstupní řez se vytváří jednou měsíčně a vlastnost scheduler hello aktivity hello nastavena také toomonthly.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-245">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="ac7a8-246">nastavení Hello hello výstupní datovou sadu a hello aktivitu Plánovač se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-246">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="ac7a8-247">Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-247">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="ac7a8-248">Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-248">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="ac7a8-249">na konci hello v této části jsou vysvětleny Hello vlastnosti používané ve hello následující JSON.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-249">hello properties used in hello following JSON are explained at hello end of this section.</span></span>

1. <span data-ttu-id="ac7a8-250">Vytvořte soubor JSON s názvem MyFirstPipelinePSH.json ve složce C:\ADFGetStarted hello s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-250">Create a JSON file named MyFirstPipelinePSH.json in hello C:\ADFGetStarted folder with hello following content:</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="ac7a8-251">Nahraďte **storageaccountname** hello název účtu úložiště v hello JSON.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-251">Replace **storageaccountname** with hello name of your storage account in hello JSON.</span></span>
   >
   >

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
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
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```
    <span data-ttu-id="ac7a8-252">V hello fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, který používá Hive tooprocess Data v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-252">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="ac7a8-253">soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **StorageLinkedService**) a v **skriptu ** složky v kontejneru hello **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-253">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **StorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

    <span data-ttu-id="ac7a8-254">Hello **definuje** část nastavení použité toospecify hello běhového prostředí, které se předá skriptu hive toohello jako konfigurační hodnoty Hive (např. ${hiveconf: inputtable}, ${určuje}).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-254">hello **defines** section is used toospecify hello runtime settings that be passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="ac7a8-255">Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-255">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

    <span data-ttu-id="ac7a8-256">V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-256">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ac7a8-257">V části "JSON kanálu" v [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md) podrobné informace o vlastnostech JSON, které se používají v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-257">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties that are used in hello example.</span></span>

2. <span data-ttu-id="ac7a8-258">Zkontrolujte, jestli hello **input.log** souboru v hello **adfgetstarted/inputdata** složky v hello úložiště objektů blob v Azure a spusťte hello následující příkaz toodeploy hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-258">Confirm that you see hello **input.log** file in hello **adfgetstarted/inputdata** folder in hello Azure blob storage, and run hello following command toodeploy hello pipeline.</span></span> <span data-ttu-id="ac7a8-259">Od hello **spustit** a **end** jsou nastavené v posledních hello a **isPaused** je sada toofalse, hello kanál (aktivita v kanálu hello) spustí hned po nasazení.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-259">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
    ```
3. <span data-ttu-id="ac7a8-260">Úspěšně jste vytvořili první kanál pomocí prostředí Azure PowerShell, blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="ac7a8-260">Congratulations, you have successfully created your first pipeline using Azure PowerShell!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="ac7a8-261">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="ac7a8-261">Monitor pipeline</span></span>
<span data-ttu-id="ac7a8-262">V tomto kroku budete pomocí prostředí Azure PowerShell toomonitor co se děje v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-262">In this step, you use Azure PowerShell toomonitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="ac7a8-263">Spustit **Get-AzureRmDataFactory** a přiřaďte výstup tooa hello **$df** proměnné.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-263">Run **Get-AzureRmDataFactory** and assign hello output tooa **$df** variable.</span></span>

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
2. <span data-ttu-id="ac7a8-264">Spustit **Get-AzureRmDataFactorySlice** tooget podrobnosti o všech řezech tabulky hello **EmpSQLTable**, což je výstupní tabulkou kanálu hello hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-264">Run **Get-AzureRmDataFactorySlice** tooget details about all slices of hello **EmpSQLTable**, which is hello output table of hello pipeline.</span></span>

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```
    <span data-ttu-id="ac7a8-265">Všimněte si, že hello StartDateTime zde určíte je stejná jako počáteční čas uvedený v kódu JSON kanálu hello hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-265">Notice that hello StartDateTime you specify here is hello same start time specified in hello pipeline JSON.</span></span> <span data-ttu-id="ac7a8-266">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-266">Here is hello sample output:</span></span>

    ```PowerShell
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : FirstDataFactoryPSH
    DatasetName       : AzureBlobOutput
    Start             : 7/1/2017 12:00:00 AM
    End               : 7/2/2017 12:00:00 AM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. <span data-ttu-id="ac7a8-267">Spustit **Get-AzureRmDataFactoryRun** tooget hello podrobnosti aktivity spouští pro určitý řez.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-267">Run **Get-AzureRmDataFactoryRun** tooget hello details of activity runs for a specific slice.</span></span>

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```

    <span data-ttu-id="ac7a8-268">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-268">Here is hello sample output:</span></span> 

    ```PowerShell
    Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : FirstDataFactoryPSH
    DatasetName         : AzureBlobOutput
    ProcessingStartTime : 12/18/2015 4:50:33 AM
    ProcessingEndTime   : 12/31/9999 11:59:59 PM
    PercentComplete     : 0
    DataSliceStart      : 7/1/2017 12:00:00 AM
    DataSliceEnd        : 7/2/2017 12:00:00 AM
    Status              : AllocatingResources
    Timestamp           : 12/18/2015 4:50:33 AM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : RunSampleHiveActivity
    PipelineName        : MyFirstPipeline
    Type                : Script
    ```
    <span data-ttu-id="ac7a8-269">Můžete spouštět tuto rutinu dokud neuvidíte hello řez ve **připraven** stavu nebo **se nezdařilo** stavu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-269">You can keep running this cmdlet until you see hello slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="ac7a8-270">Když hello řez ve stavu Připraveno, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-270">When hello slice is in Ready state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  <span data-ttu-id="ac7a8-271">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-271">Creation of an on-demand HDInsight cluster usually takes some time.</span></span>

    ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)

> [!IMPORTANT]
> <span data-ttu-id="ac7a8-273">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-273">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="ac7a8-274">Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-274">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
>
> <span data-ttu-id="ac7a8-275">vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-275">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="ac7a8-276">Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-276">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

## <a name="summary"></a><span data-ttu-id="ac7a8-277">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ac7a8-277">Summary</span></span>
<span data-ttu-id="ac7a8-278">V tomto kurzu jste vytvořili Azure data factory tooprocess data pomocí skriptu Hive v clusteru HDInsight hadoop.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-278">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="ac7a8-279">Jste použili hello editoru služby Data Factory v hello Azure portálu toodo hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-279">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>

1. <span data-ttu-id="ac7a8-280">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-280">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="ac7a8-281">Vytvořili jste dvě **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="ac7a8-281">Created two **linked services**:</span></span>
   1. <span data-ttu-id="ac7a8-282">**Úložiště Azure** propojené služby toolink služby Azure blob storage, který obsahuje vstupní/výstupní soubory toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-282">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="ac7a8-283">**Azure HDInsight** na vyžádání propojené služby toolink služby na vyžádání HDInsight Hadoop clusteru toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-283">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="ac7a8-284">Azure Data Factory vytvoří HDInsight Hadoop clusteru za běhu tooprocess vstupní data a výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-284">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="ac7a8-285">Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-285">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="ac7a8-286">Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-286">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac7a8-287">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac7a8-287">Next steps</span></span>
<span data-ttu-id="ac7a8-288">V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru Azure HDInsight na vyžádání spouští skript Hive.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-288">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="ac7a8-289">jak zjistit, toouse toocopy aktivity kopírování dat z tooAzure objektů Blob v Azure SQL, toosee [kurz: kopírování dat ze objektů Blob v Azure tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ac7a8-289">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="ac7a8-290">Viz také</span><span class="sxs-lookup"><span data-stu-id="ac7a8-290">See Also</span></span>
| <span data-ttu-id="ac7a8-291">Téma</span><span class="sxs-lookup"><span data-stu-id="ac7a8-291">Topic</span></span> | <span data-ttu-id="ac7a8-292">Popis</span><span class="sxs-lookup"><span data-stu-id="ac7a8-292">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="ac7a8-293">Referenční informace o rutinách služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="ac7a8-293">Data Factory Cmdlet Reference</span></span>](/powershell/module/azurerm.datafactories) |<span data-ttu-id="ac7a8-294">Tady najdete rozsáhlou dokumentaci o rutinách služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-294">See comprehensive documentation on Data Factory cmdlets</span></span> |
| [<span data-ttu-id="ac7a8-295">Kanály</span><span class="sxs-lookup"><span data-stu-id="ac7a8-295">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="ac7a8-296">Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-296">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="ac7a8-297">Datové sady</span><span class="sxs-lookup"><span data-stu-id="ac7a8-297">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="ac7a8-298">Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-298">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="ac7a8-299">Plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="ac7a8-299">Scheduling and Execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="ac7a8-300">Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-300">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="ac7a8-301">Monitorování a správa kanálů pomocí monitorovací aplikace</span><span class="sxs-lookup"><span data-stu-id="ac7a8-301">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="ac7a8-302">Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac7a8-302">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
