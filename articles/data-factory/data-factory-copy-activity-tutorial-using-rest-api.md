---
title: "Kurz: Použití rozhraní REST API toocreate kanál služby Azure Data Factory | Microsoft Docs"
description: "V tomto kurzu použijete rozhraní API REST toocreate kanál služby Azure Data Factory s aktivitou kopírování toocopy dat z Azure blob storage Azure SQL database."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: aa6c9b035101c4ff9acff90117ca6e3e7067f418
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-rest-api-toocreate-an-azure-data-factory-pipeline-toocopy-data"></a><span data-ttu-id="0682a-103">Kurz: Toocreate pomocí REST API Azure Data Factory kanálu toocopy dat</span><span class="sxs-lookup"><span data-stu-id="0682a-103">Tutorial: Use REST API toocreate an Azure Data Factory pipeline toocopy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0682a-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="0682a-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="0682a-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="0682a-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="0682a-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0682a-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="0682a-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0682a-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="0682a-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0682a-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="0682a-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="0682a-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="0682a-110">REST API</span><span class="sxs-lookup"><span data-stu-id="0682a-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="0682a-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="0682a-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="0682a-112">V tomto článku se dozvíte, jak toouse REST API toocreate objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="0682a-112">In this article, you learn how toouse REST API toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="0682a-113">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0682a-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="0682a-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="0682a-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="0682a-115">Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="0682a-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="0682a-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="0682a-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="0682a-117">Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="0682a-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="0682a-118">Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="0682a-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="0682a-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="0682a-120">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="0682a-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="0682a-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="0682a-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="0682a-122">Tento článek nepopisuje všechny hello REST API služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0682a-122">This article does not cover all hello Data Factory REST API.</span></span> <span data-ttu-id="0682a-123">Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Rozhraní REST API služby Data Factory - referenční informace](/rest/api/datafactory/).</span><span class="sxs-lookup"><span data-stu-id="0682a-123">See [Data Factory REST API Reference](/rest/api/datafactory/) for comprehensive documentation on Data Factory cmdlets.</span></span>
>  
> <span data-ttu-id="0682a-124">Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="0682a-124">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="0682a-125">Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-125">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0682a-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0682a-126">Prerequisites</span></span>
* <span data-ttu-id="0682a-127">Projděte [přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="0682a-127">Go through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="0682a-128">Nainstalujte na svůj počítač nástroj [Curl](https://curl.haxx.se/dlwiz/).</span><span class="sxs-lookup"><span data-stu-id="0682a-128">Install [Curl](https://curl.haxx.se/dlwiz/) on your machine.</span></span> <span data-ttu-id="0682a-129">Použijete nástroj Curl hello s toocreate příkazů REST služby data factory.</span><span class="sxs-lookup"><span data-stu-id="0682a-129">You use hello Curl tool with REST commands toocreate a data factory.</span></span> 
* <span data-ttu-id="0682a-130">Postupujte podle pokynů v [tomto článku](../azure-resource-manager/resource-group-create-service-principal-portal.md) a proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="0682a-130">Follow instructions from [this article](../azure-resource-manager/resource-group-create-service-principal-portal.md) to:</span></span> 
  1. <span data-ttu-id="0682a-131">V Azure Active Directory vytvořte webovou aplikaci s názvem **ADFCopyTutorialApp**.</span><span class="sxs-lookup"><span data-stu-id="0682a-131">Create a Web application named **ADFCopyTutorialApp** in Azure Active Directory.</span></span>
  2. <span data-ttu-id="0682a-132">Získejte **ID klienta** a **tajný klíč**.</span><span class="sxs-lookup"><span data-stu-id="0682a-132">Get **client ID** and **secret key**.</span></span> 
  3. <span data-ttu-id="0682a-133">Získejte **ID tenanta**.</span><span class="sxs-lookup"><span data-stu-id="0682a-133">Get **tenant ID**.</span></span> 
  4. <span data-ttu-id="0682a-134">Přiřadit hello **ADFCopyTutorialApp** aplikace toohello **Data Factory Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="0682a-134">Assign hello **ADFCopyTutorialApp** application toohello **Data Factory Contributor** role.</span></span>  
* <span data-ttu-id="0682a-135">Nainstalujte [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0682a-135">Install [Azure PowerShell](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="0682a-136">Spusťte **prostředí PowerShell** a hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="0682a-136">Launch **PowerShell** and do hello following steps.</span></span> <span data-ttu-id="0682a-137">Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0682a-137">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="0682a-138">Pokud zavřete a znovu otevřete, toorun hello příkazů je třeba znovu.</span><span class="sxs-lookup"><span data-stu-id="0682a-138">If you close and reopen, you need toorun hello commands again.</span></span>
  
  1. <span data-ttu-id="0682a-139">Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="0682a-139">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal:</span></span>
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. <span data-ttu-id="0682a-140">Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet:</span><span class="sxs-lookup"><span data-stu-id="0682a-140">Run hello following command tooview all hello subscriptions for this account:</span></span>

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. <span data-ttu-id="0682a-141">Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-141">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="0682a-142">Nahraďte  **&lt;NameOfAzureSubscription** &gt; s názvem hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="0682a-142">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span> 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. <span data-ttu-id="0682a-143">Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** tak, že spustíte následující příkaz v hello prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-143">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell:</span></span>  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      <span data-ttu-id="0682a-144">Pokud skupina prostředků hello již existuje, zadejte zda tooupdate ho (Y) nebo jej zachovat jako (ne).</span><span class="sxs-lookup"><span data-stu-id="0682a-144">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span> 
     
      <span data-ttu-id="0682a-145">Některé hello kroky v tomto kurzu vychází z předpokladu, že používáte hello skupinu prostředků s názvem ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="0682a-145">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="0682a-146">Pokud používáte jiné skupině prostředků, musíte v tomto kurzu toouse hello název vaší skupiny prostředků místo skupiny ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="0682a-146">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>

## <a name="create-json-definitions"></a><span data-ttu-id="0682a-147">Vytvoření definic JSON</span><span class="sxs-lookup"><span data-stu-id="0682a-147">Create JSON definitions</span></span>
<span data-ttu-id="0682a-148">Vytvořte následující soubory JSON ve složce hello, kde se nachází curl.exe.</span><span class="sxs-lookup"><span data-stu-id="0682a-148">Create following JSON files in hello folder where curl.exe is located.</span></span> 

### <a name="datafactoryjson"></a><span data-ttu-id="0682a-149">datafactory.json</span><span class="sxs-lookup"><span data-stu-id="0682a-149">datafactory.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0682a-150">Název musí být globálně jedinečné, proto je vhodné toomake ADFCopyTutorialDF tooprefix či přípon je jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="0682a-150">Name must be globally unique, so you may want tooprefix/suffix ADFCopyTutorialDF toomake it a unique name.</span></span> 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a><span data-ttu-id="0682a-151">azurestoragelinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="0682a-151">azurestoragelinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0682a-152">Položky **accountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="0682a-152">Replace **accountname** and **accountkey** with name and key of your Azure storage account.</span></span> <span data-ttu-id="0682a-153">toolearn tooget úložiště přístupu klíče najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="0682a-153">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>

```JSON
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

<span data-ttu-id="0682a-154">Podrobné informace o vlastnostech JSON najdete v tématu [Propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span><span class="sxs-lookup"><span data-stu-id="0682a-154">For details about JSON properties, see [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span></span>

### <a name="azuersqllinkedservicejson"></a><span data-ttu-id="0682a-155">azuersqllinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="0682a-155">azuersqllinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0682a-156">Nahraďte **servername**, **databasename**, **uživatelské jméno**, a **heslo** s názvem serveru Azure SQL, název databáze SQL uživatele účet a heslo pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-156">Replace **servername**, **databasename**, **username**, and **password** with name of your Azure SQL server, name of SQL database, user account, and password for hello account.</span></span>  
> 
>

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

<span data-ttu-id="0682a-157">Podrobné informace o vlastnostech JSON najdete v tématu [Propojená služba Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0682a-157">For details about JSON properties, see [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>

### <a name="inputdatasetjson"></a><span data-ttu-id="0682a-158">inputdataset.json</span><span class="sxs-lookup"><span data-stu-id="0682a-158">inputdataset.json</span></span>

```JSON
{
  "name": "AzureBlobInput",
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
      "folderPath": "adftutorial/",
      "fileName": "emp.txt",
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

<span data-ttu-id="0682a-159">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-159">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="0682a-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0682a-160">Property</span></span> | <span data-ttu-id="0682a-161">Popis</span><span class="sxs-lookup"><span data-stu-id="0682a-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0682a-162">type</span><span class="sxs-lookup"><span data-stu-id="0682a-162">type</span></span> | <span data-ttu-id="0682a-163">Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="0682a-163">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
| <span data-ttu-id="0682a-164">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="0682a-164">linkedServiceName</span></span> | <span data-ttu-id="0682a-165">Odkazuje toohello **AzureStorageLinkedService** kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0682a-165">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="0682a-166">folderPath</span><span class="sxs-lookup"><span data-stu-id="0682a-166">folderPath</span></span> | <span data-ttu-id="0682a-167">Určuje objekt blob hello **kontejneru** a hello **složky** , který obsahuje vstupní objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="0682a-167">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="0682a-168">V tomto kurzu adftutorial hello kontejner objektů blob, složka hello kořenové složky.</span><span class="sxs-lookup"><span data-stu-id="0682a-168">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
| <span data-ttu-id="0682a-169">fileName</span><span class="sxs-lookup"><span data-stu-id="0682a-169">fileName</span></span> | <span data-ttu-id="0682a-170">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="0682a-170">This property is optional.</span></span> <span data-ttu-id="0682a-171">Pokud ji vynecháte, jsou vybrány všechny soubory z hello folderPath.</span><span class="sxs-lookup"><span data-stu-id="0682a-171">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="0682a-172">V tomto kurzu **emp.txt** zadaný pro hello název souboru, takže pouze tento soubor je převzata pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="0682a-172">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
| <span data-ttu-id="0682a-173">format -> type</span><span class="sxs-lookup"><span data-stu-id="0682a-173">format -> type</span></span> |<span data-ttu-id="0682a-174">vstupní soubor Hello je ve formátu textu hello, takže použijeme **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="0682a-174">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
| <span data-ttu-id="0682a-175">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="0682a-175">columnDelimiter</span></span> | <span data-ttu-id="0682a-176">Hello sloupců ve vstupním souboru hello oddělené **čárku (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="0682a-176">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
| <span data-ttu-id="0682a-177">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="0682a-177">frequency/interval</span></span> | <span data-ttu-id="0682a-178">je příliš nastavena frekvence Hello**hodinu** a interval je nastaven příliš**1**, což znamená, že hello vstupní řezy jsou dostupné **každou hodinu**.</span><span class="sxs-lookup"><span data-stu-id="0682a-178">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="0682a-179">Jinými slovy, hello služba Data Factory hledá vstupní data každou hodinu v kořenové složce hello kontejner objektů blob (**adftutorial**) jste zadali.</span><span class="sxs-lookup"><span data-stu-id="0682a-179">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="0682a-180">Hledá hello dat v rámci hello kanálu počáteční a koncové dobu, není před nebo po této doby.</span><span class="sxs-lookup"><span data-stu-id="0682a-180">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
| <span data-ttu-id="0682a-181">external</span><span class="sxs-lookup"><span data-stu-id="0682a-181">external</span></span> | <span data-ttu-id="0682a-182">Tato vlastnost nastavena příliš**true** Pokud hello data nevygenerovala pomocí tohoto kanálu.</span><span class="sxs-lookup"><span data-stu-id="0682a-182">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="0682a-183">Hello vstupní data v tomto kurzu je v souboru emp.txt hello, který není generované tímto kanálem, abyste nám nastavit tuto vlastnost tootrue.</span><span class="sxs-lookup"><span data-stu-id="0682a-183">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

<span data-ttu-id="0682a-184">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0682a-184">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>

### <a name="outputdatasetjson"></a><span data-ttu-id="0682a-185">outputdataset.json</span><span class="sxs-lookup"><span data-stu-id="0682a-185">outputdataset.json</span></span>

```JSON
{
  "name": "AzureSqlOutput",
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
<span data-ttu-id="0682a-186">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-186">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="0682a-187">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0682a-187">Property</span></span> | <span data-ttu-id="0682a-188">Popis</span><span class="sxs-lookup"><span data-stu-id="0682a-188">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0682a-189">type</span><span class="sxs-lookup"><span data-stu-id="0682a-189">type</span></span> | <span data-ttu-id="0682a-190">Hello vlastnost Typ nastavena příliš**AzureSqlTable** protože data jsou zkopírované tooa tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="0682a-190">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
| <span data-ttu-id="0682a-191">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="0682a-191">linkedServiceName</span></span> | <span data-ttu-id="0682a-192">Odkazuje toohello **AzureSqlLinkedService** kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0682a-192">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="0682a-193">tableName</span><span class="sxs-lookup"><span data-stu-id="0682a-193">tableName</span></span> | <span data-ttu-id="0682a-194">Zadaný hello **tabulky** toowhich hello data budou zkopírována.</span><span class="sxs-lookup"><span data-stu-id="0682a-194">Specified hello **table** toowhich hello data is copied.</span></span> | 
| <span data-ttu-id="0682a-195">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="0682a-195">frequency/interval</span></span> | <span data-ttu-id="0682a-196">Hello je nastavena frekvence příliš**hodinu** a interval je **1**, což znamená, že vytváří řezy výstup hello **každou hodinu** mezi hello kanálu počáteční a koncové krát, ne dřív nebo Po této doby.</span><span class="sxs-lookup"><span data-stu-id="0682a-196">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

<span data-ttu-id="0682a-197">Existují tři sloupce – **ID**, **FirstName**, a **LastName** – v tabulce emp hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-197">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="0682a-198">ID je sloupec identity, takže je nutné pouze toospecify **FirstName** a **LastName** sem.</span><span class="sxs-lookup"><span data-stu-id="0682a-198">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

<span data-ttu-id="0682a-199">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0682a-199">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

### <a name="pipelinejson"></a><span data-ttu-id="0682a-200">pipeline.json</span><span class="sxs-lookup"><span data-stu-id="0682a-200">pipeline.json</span></span>

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data tooAzure SQL database",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
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

<span data-ttu-id="0682a-201">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="0682a-201">Note hello following points:</span></span>

- <span data-ttu-id="0682a-202">V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**.</span><span class="sxs-lookup"><span data-stu-id="0682a-202">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="0682a-203">Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-203">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="0682a-204">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-204">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
- <span data-ttu-id="0682a-205">Vstup aktivity hello nastaven příliš**AzureBlobInput** a výstup hello aktivity je nastavený příliš**AzureSqlOutput**.</span><span class="sxs-lookup"><span data-stu-id="0682a-205">Input for hello activity is set too**AzureBlobInput** and output for hello activity is set too**AzureSqlOutput**.</span></span> 
- <span data-ttu-id="0682a-206">V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-206">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="0682a-207">Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="0682a-207">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="0682a-208">toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-208">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
 
<span data-ttu-id="0682a-209">Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den.</span><span class="sxs-lookup"><span data-stu-id="0682a-209">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="0682a-210">Můžete zadat jenom část data hello a přeskočit část času hello hello datum a čas.</span><span class="sxs-lookup"><span data-stu-id="0682a-210">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="0682a-211">Například "2017-02-03", který je ekvivalentní příliš "2017-02-03T00:00:00Z"</span><span class="sxs-lookup"><span data-stu-id="0682a-211">For example, "2017-02-03", which is equivalent too"2017-02-03T00:00:00Z"</span></span>
 
<span data-ttu-id="0682a-212">Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="0682a-212">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="0682a-213">Například: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="0682a-213">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="0682a-214">Hello **end** čas je nepovinný, ale používáme v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0682a-214">hello **end** time is optional, but we use it in this tutorial.</span></span> 
 
<span data-ttu-id="0682a-215">Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**".</span><span class="sxs-lookup"><span data-stu-id="0682a-215">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="0682a-216">bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0682a-216">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
 
<span data-ttu-id="0682a-217">V předchozím příkladu hello jsou 24 datových řezů jako se vytvářejí každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="0682a-217">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

<span data-ttu-id="0682a-218">Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-218">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="0682a-219">Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-219">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="0682a-220">Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-220">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="0682a-221">Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-221">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="set-global-variables"></a><span data-ttu-id="0682a-222">Nastavení globálních proměnných</span><span class="sxs-lookup"><span data-stu-id="0682a-222">Set global variables</span></span>
<span data-ttu-id="0682a-223">V prostředí Azure PowerShell spusťte následující příkazy po nahrazení hello hodnoty vlastními hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-223">In Azure PowerShell, execute hello following commands after replacing hello values with your own:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0682a-224">Postup získání ID klienta, tajného klíče klienta, ID tenanta a ID předplatného naleznete v sekci [Požadavky](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="0682a-224">See [Prerequisites](#prerequisites) section for instructions on getting client ID, client secret, tenant ID, and subscription ID.</span></span>   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
```

<span data-ttu-id="0682a-225">Spusťte následující příkaz po aktualizaci hello název objektu pro vytváření dat hello, který používáte hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-225">Run hello following command after updating hello name of hello data factory you are using:</span></span> 

```
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a><span data-ttu-id="0682a-226">Ověření pomocí ADD</span><span class="sxs-lookup"><span data-stu-id="0682a-226">Authenticate with AAD</span></span>
<span data-ttu-id="0682a-227">Spusťte následující příkaz tooauthenticate s Azure Active Directory (AAD) hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-227">Run hello following command tooauthenticate with Azure Active Directory (AAD):</span></span> 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a><span data-ttu-id="0682a-228">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="0682a-228">Create data factory</span></span>
<span data-ttu-id="0682a-229">V tomto kroku vytvoříte objekt služby Azure Data Factory s názvem **ADFCopyTutorialDF**.</span><span class="sxs-lookup"><span data-stu-id="0682a-229">In this step, you create an Azure Data Factory named **ADFCopyTutorialDF**.</span></span> <span data-ttu-id="0682a-230">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="0682a-230">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="0682a-231">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="0682a-231">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="0682a-232">Například úložiště dat toocopy aktivity kopírování ze zdroje dat cílový tooa.</span><span class="sxs-lookup"><span data-stu-id="0682a-232">For example, a Copy Activity toocopy data from a source tooa destination data store.</span></span> <span data-ttu-id="0682a-233">Toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0682a-233">A HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="0682a-234">Spusťte následující příkazy toocreate hello objekt pro vytváření dat hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-234">Run hello following commands toocreate hello data factory:</span></span> 

1. <span data-ttu-id="0682a-235">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0682a-235">Assign hello command toovariable named **cmd**.</span></span> 
   
    > [!IMPORTANT]
    > <span data-ttu-id="0682a-236">Potvrďte, že hello název objektu pro vytváření dat hello zde určíte název zadaný v hello hello (ADFCopyTutorialDF) odpovídá **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="0682a-236">Confirm that hello name of hello data factory you specify here (ADFCopyTutorialDF) matches hello name specified in hello **datafactory.json**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF0411?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0682a-237">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0682a-237">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0682a-238">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-238">View hello results.</span></span> <span data-ttu-id="0682a-239">Pokud objekt pro vytváření dat hello byl úspěšně vytvořen, zobrazí hello JSON pro vytváření dat hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0682a-239">If hello data factory has been successfully created, you see hello JSON for hello data factory in hello **results**; otherwise, you see an error message.</span></span>  
   
    ```
    Write-Host $results
    ```

<span data-ttu-id="0682a-240">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="0682a-240">Note hello following points:</span></span>

* <span data-ttu-id="0682a-241">Název Hello hello objekt pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="0682a-241">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="0682a-242">Pokud se zobrazí chyba hello ve výsledcích: **název objektu pro vytváření dat "ADFCopyTutorialDF" není k dispozici**, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0682a-242">If you see hello error in results: **Data factory name “ADFCopyTutorialDF” is not available**, do hello following steps:</span></span>  
  
  1. <span data-ttu-id="0682a-243">Změna hello název (například yournameADFCopyTutorialDF) v hello **datafactory.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="0682a-243">Change hello name (for example, yournameADFCopyTutorialDF) in hello **datafactory.json** file.</span></span>
  2. <span data-ttu-id="0682a-244">V první příkaz hello kde hello **$cmd** přiřazena hodnota proměnné, nahraďte ADFCopyTutorialDF hello nový název a spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-244">In hello first command where hello **$cmd** variable is assigned a value, replace ADFCopyTutorialDF with hello new name and run hello command.</span></span> 
  3. <span data-ttu-id="0682a-245">Spusťte hello následující dva příkazy tooinvoke hello REST API toocreate hello data factory a tisk hello výsledky operace hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-245">Run hello next two commands tooinvoke hello REST API toocreate hello data factory and print hello results of hello operation.</span></span> 
     
     <span data-ttu-id="0682a-246">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0682a-246">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
* <span data-ttu-id="0682a-247">instance služby Data Factory toocreate, je nutné toobe přispěvatelem/správcem předplatného Azure hello</span><span class="sxs-lookup"><span data-stu-id="0682a-247">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="0682a-248">Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="0682a-248">hello name of hello data factory may be registered as a DNS name in hello future and hence become publicly visible.</span></span>
* <span data-ttu-id="0682a-249">Pokud se zobrazí chyba hello: "**toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory**", proveďte jednu z následujících hello a zkuste to znovu publikovat:</span><span class="sxs-lookup"><span data-stu-id="0682a-249">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span> 
  
  * <span data-ttu-id="0682a-250">V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-250">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="0682a-251">Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0682a-251">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="0682a-252">Přihlášení pomocí hello předplatného Azure hello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0682a-252">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="0682a-253">Tato akce automaticky registruje zprostředkovatele hello za vás.</span><span class="sxs-lookup"><span data-stu-id="0682a-253">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="0682a-254">Před vytvořením kanálu, je nutné toocreate několik entit služby Data Factory nejdřív.</span><span class="sxs-lookup"><span data-stu-id="0682a-254">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="0682a-255">Nejprve vytvoříte propojené služby toolink zdrojové a cílové data ukládá tooyour data uložit.</span><span class="sxs-lookup"><span data-stu-id="0682a-255">You first create linked services toolink source and destination data stores tooyour data store.</span></span> <span data-ttu-id="0682a-256">Potom zadejte vstupní a výstupní datové sady toorepresent data v propojených úložištích dat.</span><span class="sxs-lookup"><span data-stu-id="0682a-256">Then, define input and output datasets toorepresent data in linked data stores.</span></span> <span data-ttu-id="0682a-257">Nakonec vytvořte hello kanálu s aktivitou, která používá tyto datové sady.</span><span class="sxs-lookup"><span data-stu-id="0682a-257">Finally, create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="0682a-258">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="0682a-258">Create linked services</span></span>
<span data-ttu-id="0682a-259">Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="0682a-259">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="0682a-260">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0682a-260">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="0682a-261">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="0682a-261">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> <span data-ttu-id="0682a-262">Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="0682a-262">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="0682a-263">Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="0682a-263">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="0682a-264">Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-264">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="0682a-265">AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="0682a-265">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="0682a-266">Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi.</span><span class="sxs-lookup"><span data-stu-id="0682a-266">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="0682a-267">Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="0682a-267">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="0682a-268">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0682a-268">Create Azure Storage linked service</span></span>
<span data-ttu-id="0682a-269">V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0682a-269">In this step, you link your Azure storage account tooyour data factory.</span></span> <span data-ttu-id="0682a-270">Zadejte název hello a klíč účtu úložiště Azure v této části.</span><span class="sxs-lookup"><span data-stu-id="0682a-270">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="0682a-271">V tématu [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="0682a-271">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span>  

1. <span data-ttu-id="0682a-272">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0682a-272">Assign hello command toovariable named **cmd**.</span></span> 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0682a-273">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0682a-273">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0682a-274">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-274">View hello results.</span></span> <span data-ttu-id="0682a-275">Pokud hello propojené služby byla úspěšně vytvořena, najdete v části hello JSON hello propojené služby hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0682a-275">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a><span data-ttu-id="0682a-276">Vytvoření propojené služby Azure SQL</span><span class="sxs-lookup"><span data-stu-id="0682a-276">Create Azure SQL linked service</span></span>
<span data-ttu-id="0682a-277">V tomto kroku propojíte datovou továrnu tooyour databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="0682a-277">In this step, you link your Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="0682a-278">Zadejte název serveru Azure SQL hello, název databáze, uživatelské jméno a heslo uživatele v této části.</span><span class="sxs-lookup"><span data-stu-id="0682a-278">You specify hello Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="0682a-279">V tématu [propojená služba Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) podrobnosti o toodefine vlastnosti používané JSON Azure SQL propojené služby.</span><span class="sxs-lookup"><span data-stu-id="0682a-279">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used toodefine an Azure SQL linked service.</span></span>

1. <span data-ttu-id="0682a-280">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0682a-280">Assign hello command toovariable named **cmd**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0682a-281">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0682a-281">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0682a-282">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-282">View hello results.</span></span> <span data-ttu-id="0682a-283">Pokud hello propojené služby byla úspěšně vytvořena, najdete v části hello JSON hello propojené služby hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0682a-283">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a><span data-ttu-id="0682a-284">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="0682a-284">Create datasets</span></span>
<span data-ttu-id="0682a-285">V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="0682a-285">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="0682a-286">V tomto kroku definujete dvě datové sady s názvem AzureBlobInput a AzureSqlOutput, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService a AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="0682a-286">In this step, you define two datasets named AzureBlobInput and AzureSqlOutput that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="0682a-287">Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0682a-287">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="0682a-288">A datovou sadu hello vstupního objektu blob (AzureBlobInput) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-288">And, hello input blob dataset (AzureBlobInput) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="0682a-289">Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="0682a-289">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="0682a-290">A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-290">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="0682a-291">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="0682a-291">Create input dataset</span></span>
<span data-ttu-id="0682a-292">V tomto kroku vytvoříte datové sady s názvem AzureBlobInput, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService propojené služby.</span><span class="sxs-lookup"><span data-stu-id="0682a-292">In this step, you create a dataset named AzureBlobInput that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="0682a-293">Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="0682a-293">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="0682a-294">V tomto kurzu zadejte hodnotu pro název souboru hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-294">In this tutorial, you specify a value for hello fileName.</span></span> 

1. <span data-ttu-id="0682a-295">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0682a-295">Assign hello command toovariable named **cmd**.</span></span> 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0682a-296">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0682a-296">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0682a-297">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-297">View hello results.</span></span> <span data-ttu-id="0682a-298">Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0682a-298">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="0682a-299">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="0682a-299">Create output dataset</span></span>
<span data-ttu-id="0682a-300">Hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="0682a-300">hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="0682a-301">Hello výstupní SQL tabulky datové sady (OututDataset) v tomto kroku vytvoříte Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-301">hello output SQL table dataset (OututDataset) you create in this step specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>

1. <span data-ttu-id="0682a-302">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0682a-302">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0682a-303">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0682a-303">Run hello command by using **Invoke-Command**.</span></span>
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0682a-304">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-304">View hello results.</span></span> <span data-ttu-id="0682a-305">Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0682a-305">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a><span data-ttu-id="0682a-306">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="0682a-306">Create pipeline</span></span>
<span data-ttu-id="0682a-307">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **AzureBlobInput** jako vstup a **AzureSqlOutput** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="0682a-307">In this step, you create a pipeline with a **copy activity** that uses **AzureBlobInput** as an input and **AzureSqlOutput** as an output.</span></span>

<span data-ttu-id="0682a-308">Výstupní datové sady v současné době je, jaké jednotky hello plán.</span><span class="sxs-lookup"><span data-stu-id="0682a-308">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="0682a-309">V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="0682a-309">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="0682a-310">Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="0682a-310">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="0682a-311">Proto 24 řezů výstupní datové sady vyprodukované hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="0682a-311">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 

1. <span data-ttu-id="0682a-312">Přiřadit toovariable hello příkaz s názvem **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0682a-312">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0682a-313">Spusťte příkaz hello pomocí **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0682a-313">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0682a-314">Zobrazení výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-314">View hello results.</span></span> <span data-ttu-id="0682a-315">Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0682a-315">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>  

    ```PowerShell   
    Write-Host $results
    ```

<span data-ttu-id="0682a-316">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="0682a-316">**Congratulations!**</span></span> <span data-ttu-id="0682a-317">Úspěšně jste vytvořili objekt pro vytváření dat Azure, s kanál, který kopíruje data z databáze SQL Azure Blob Storage tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0682a-317">You have successfully created an Azure data factory, with a pipeline that copies data from Azure Blob Storage tooAzure SQL database.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="0682a-318">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="0682a-318">Monitor pipeline</span></span>
<span data-ttu-id="0682a-319">V tomto kroku použijete řezy toomonitor REST API služby Data Factory vytvářen hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="0682a-319">In this step, you use Data Factory REST API toomonitor slices being produced by hello pipeline.</span></span>

```PowerShell
$ds ="AzureSqlOutput"
```

> [!IMPORTANT] 
> <span data-ttu-id="0682a-320">Ujistěte se, že hello počáteční a koncové časy zadané v hello následující příkaz shodu hello spuštění a ukončení hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="0682a-320">Make sure that hello start and end times specified in hello following command match hello start and end times of hello pipeline.</span></span> 

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=2017-05-11T00%3a00%3a00.0000000Z"&"end=2017-05-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

<span data-ttu-id="0682a-321">Spustit hello Invoke-Command a hello dalšímu, dokud neuvidíte řez v **připraven** stavu nebo **se nezdařilo** stavu.</span><span class="sxs-lookup"><span data-stu-id="0682a-321">Run hello Invoke-Command and hello next one until you see a slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="0682a-322">Když hello řez ve stavu Připraveno, zkontrolujte hello **emp** tabulky v databázi Azure SQL pro hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0682a-322">When hello slice is in Ready state, check hello **emp** table in your Azure SQL database for hello output data.</span></span> 

<span data-ttu-id="0682a-323">Pro každý řez jsou dva řádky dat ze zdrojového souboru hello zkopírovaný toohello tabulce emp v databázi Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-323">For each slice, two rows of data from hello source file are copied toohello emp table in hello Azure SQL database.</span></span> <span data-ttu-id="0682a-324">Proto byste vidět 24 nové záznamy v tabulce emp hello při všech řezech hello úspěšně zpracování (ve stavu Připraveno).</span><span class="sxs-lookup"><span data-stu-id="0682a-324">Therefore, you see 24 new records in hello emp table when all hello slices are successfully processed (in Ready state).</span></span> 

## <a name="summary"></a><span data-ttu-id="0682a-325">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0682a-325">Summary</span></span>
<span data-ttu-id="0682a-326">V tomto kurzu jste použili toocreate REST API služby Azure data factory toocopy data z Azure SQL database tooan objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="0682a-326">In this tutorial, you used REST API toocreate an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="0682a-327">Zde jsou hello kroků, které jste provedli v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="0682a-327">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="0682a-328">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="0682a-328">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="0682a-329">Vytvořili jste **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="0682a-329">Created **linked services**:</span></span>
   1. <span data-ttu-id="0682a-330">Azure Storage propojené služby toolink účtu úložiště Azure, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="0682a-330">An Azure Storage linked service toolink your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="0682a-331">Azure SQL propojené toolink služby Azure SQL database, který obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0682a-331">An Azure SQL linked service toolink your Azure SQL database that holds hello output data.</span></span> 
3. <span data-ttu-id="0682a-332">Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.</span><span class="sxs-lookup"><span data-stu-id="0682a-332">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="0682a-333">Vytvořili jste **kanál** s aktivitou kopírování, která má jako zdroj BlobSource a jako jímku SqlSink.</span><span class="sxs-lookup"><span data-stu-id="0682a-333">Created a **pipeline** with a Copy Activity with BlobSource as source and SqlSink as sink.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0682a-334">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0682a-334">Next steps</span></span>
<span data-ttu-id="0682a-335">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="0682a-335">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="0682a-336">Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello:</span><span class="sxs-lookup"><span data-stu-id="0682a-336">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="0682a-337">toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="0682a-337">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
