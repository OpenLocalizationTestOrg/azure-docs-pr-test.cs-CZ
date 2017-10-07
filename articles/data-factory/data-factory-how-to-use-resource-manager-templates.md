---
title: "šablony Resource Manageru aaaUse v datové továrně | Microsoft Docs"
description: "Zjistěte, jak toocreate a pomocí Azure Resource Manager šablony toocreate entit služby Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.assetid: 37724021-f55f-4e85-9206-6d4a48bda3d8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 60d5dbd29494420006aed6d5bd9a10a63c36bec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-templates-toocreate-azure-data-factory-entities"></a><span data-ttu-id="9e4a2-103">Používání šablon toocreate Azure Data Factory entit</span><span class="sxs-lookup"><span data-stu-id="9e4a2-103">Use templates toocreate Azure Data Factory entities</span></span>
## <a name="overview"></a><span data-ttu-id="9e4a2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9e4a2-104">Overview</span></span>
<span data-ttu-id="9e4a2-105">Při použití Azure Data Factory pro integraci vašim datovým potřebám sami zjistíte opakovaného použití hello stejného vzoru různých prostředích nebo implementace hello stejná úloha opakovaně v rámci hello stejné řešení.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-105">While using Azure Data Factory for your data integration needs, you may find yourself reusing hello same pattern across different environments or implementing hello same task repetitively within hello same solution.</span></span> <span data-ttu-id="9e4a2-106">Šablony vám pomohou implementovat a spravovat tyto scénáře snadno způsobem.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-106">Templates help you implement and manage these scenarios in an easy manner.</span></span> <span data-ttu-id="9e4a2-107">Šablony v Azure Data Factory jsou ideální pro scénáře, které zahrnují – opětovné použití a opakování.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-107">Templates in Azure Data Factory are ideal for scenarios that involve reusability and repetition.</span></span>

<span data-ttu-id="9e4a2-108">Vezměte v úvahu hello situaci, kde může organizace má 10 výrobních závodech napříč hello, world.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-108">Consider hello situation where an organization has 10 manufacturing plants across hello world.</span></span> <span data-ttu-id="9e4a2-109">Hello protokoly z každého zařízení jsou uloženy v databázi systému SQL Server samostatný místní.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-109">hello logs from each plant are stored in a separate on-premises SQL Server database.</span></span> <span data-ttu-id="9e4a2-110">Hello společnost chce toobuild jednoho datového skladu v cloudu hello ad-hoc analýzy.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-110">hello company wants toobuild a single data warehouse in hello cloud for ad-hoc analytics.</span></span> <span data-ttu-id="9e4a2-111">Je také chce toohave hello stejné logiky, která ale různé konfigurace pro vývoj, testování a provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-111">It also wants toohave hello same logic but different configurations for development, test, and production environments.</span></span>

<span data-ttu-id="9e4a2-112">V takovém případě úlohu potřebuje toobe opakuje uvnitř hello stejné prostředí, ale s různými hodnotami napříč hello 10 datové továrny pro každého z výrobních závodů.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-112">In this case, a task needs toobe repeated within hello same environment, but with different values across hello 10 data factories for each manufacturing plant.</span></span> <span data-ttu-id="9e4a2-113">Ve skutečnosti **opakování** je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-113">In effect, **repetition** is present.</span></span> <span data-ttu-id="9e4a2-114">Ukázka umožňuje hello abstrakce tento obecný tok (tedy kanálů s hello stejné aktivity v každém objektu pro vytváření dat), ale používá soubor samostatného parametru pro každého z výrobních závodů.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-114">Templating allows hello abstraction of this generic flow (that is, pipelines having hello same activities in each data factory), but uses a separate parameter file for each manufacturing plant.</span></span>

<span data-ttu-id="9e4a2-115">Kromě toho, jak hello organizace přeje, aby toodeploy těchto 10 datové továrny vícekrát různých prostředích, šablony můžete použít **– opětovné použití** s využitím soubory samostatného parametru pro vývoj, testování, a provozní prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-115">Furthermore, as hello organization wants toodeploy these 10 data factories multiple times across different environments, templates can use this **reusability** by utilizing separate parameter files for development, test, and production environments.</span></span>

## <a name="templating-with-azure-resource-manager"></a><span data-ttu-id="9e4a2-116">Ukázka s Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9e4a2-116">Templating with Azure Resource Manager</span></span>
<span data-ttu-id="9e4a2-117">[Šablony Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#template-deployment) jsou ukázka tooachieve skvělý způsob, jak v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-117">[Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md#template-deployment) are a great way tooachieve templating in Azure Data Factory.</span></span> <span data-ttu-id="9e4a2-118">Šablony Resource Manageru definovat hello infrastruktury a konfigurace řešení Azure prostřednictvím soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-118">Resource Manager templates define hello infrastructure and configuration of your Azure solution through a JSON file.</span></span> <span data-ttu-id="9e4a2-119">Protože šablon Azure Resource Manageru pracovat s všechny/většina služeb Azure, široce použitím tooeasily spravovat všechny prostředky vaše prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-119">Because Azure Resource Manager templates work with all/most Azure services, it can be widely used tooeasily manage all resources of your Azure assets.</span></span> <span data-ttu-id="9e4a2-120">V tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) toolearn Další informace o obecně hello šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-120">See [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn more about hello Resource Manager Templates in general.</span></span>

## <a name="tutorials"></a><span data-ttu-id="9e4a2-121">Kurzy</span><span class="sxs-lookup"><span data-stu-id="9e4a2-121">Tutorials</span></span>
<span data-ttu-id="9e4a2-122">V tématu hello následující kurzy pro podrobné pokyny entit služby Data Factory toocreate pomocí šablony Resource Manageru:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-122">See hello following tutorials for step-by-step instructions toocreate Data Factory entities by using Resource Manager templates:</span></span>

* [<span data-ttu-id="9e4a2-123">Kurz: Vytvoření kanálu toocopy dat pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9e4a2-123">Tutorial: Create a pipeline toocopy data by using Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="9e4a2-124">Kurz: Vytvoření kanálu tooprocess dat pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9e4a2-124">Tutorial: Create a pipeline tooprocess data by using Azure Resource Manager template</span></span>](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a><span data-ttu-id="9e4a2-125">Data Factory šablony na Githubu</span><span class="sxs-lookup"><span data-stu-id="9e4a2-125">Data Factory templates on GitHub</span></span>
<span data-ttu-id="9e4a2-126">Podívejte se na hello následující šablony Azure rychlý start na Githubu:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-126">Check out hello following Azure quick start templates on GitHub:</span></span>

* [<span data-ttu-id="9e4a2-127">Vytvoření objektu pro vytváření dat toocopy dat z Azure Blob Storage tooAzure databáze SQL</span><span class="sxs-lookup"><span data-stu-id="9e4a2-127">Create a Data factory toocopy data from Azure Blob Storage tooAzure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [<span data-ttu-id="9e4a2-128">Vytvoření služby Data factory s aktivitou Hive v clusteru Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="9e4a2-128">Create a Data factory with Hive activity on Azure HDInsight cluster</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [<span data-ttu-id="9e4a2-129">Vytvoření objektu pro vytváření dat toocopy dat ze služby Salesforce tooAzure objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9e4a2-129">Create a Data factory toocopy data from Salesforce tooAzure Blobs</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [<span data-ttu-id="9e4a2-130">Vytvoření objektu pro vytváření dat, který je zřetězený aktivity: zkopíruje data ze serveru FTP tooAzure objekty BLOB, vyvolá skript hive na na vyžádání HDInsight clusteru tootransform hello data a zkopíruje výsledek do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="9e4a2-130">Create a Data factory that chains activities: copies data from an FTP server tooAzure Blobs, invokes a hive script on an on-demand HDInsight cluster tootransform hello data, and copies result into Azure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

<span data-ttu-id="9e4a2-131">Působí volné tooshare šablony Azure Data Factory v [Azure rychlý start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="9e4a2-131">Feel free tooshare your Azure Data Factory templates at [Azure Quick start](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="9e4a2-132">Odkazovat toohello [příspěvku průvodce](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) při vývoji šablony, které je možné sdílet přes toto úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-132">Refer toohello [contribution guide](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) while developing templates that can be shared via this repository.</span></span>

<span data-ttu-id="9e4a2-133">Hello následující části obsahují podrobné informace o definování prostředků pro vytváření dat v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-133">hello following sections provide details about defining Data Factory resources in a Resource Manager template.</span></span>

## <a name="defining-data-factory-resources-in-templates"></a><span data-ttu-id="9e4a2-134">Definice prostředků pro vytváření dat v šablonách</span><span class="sxs-lookup"><span data-stu-id="9e4a2-134">Defining Data Factory resources in templates</span></span>
<span data-ttu-id="9e4a2-135">Hello nejvyšší úrovně šablonu pro definování objekt pro vytváření dat je:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-135">hello top-level template for defining a data factory is:</span></span>

```JSON
"$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
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
    { "type": "linkedservices",
        ...
    },
    {"type": "datasets",
        ...
    },
    {"type": "dataPipelines",
        ...
    }
}
```

### <a name="define-data-factory"></a><span data-ttu-id="9e4a2-136">Definování datové továrny</span><span class="sxs-lookup"><span data-stu-id="9e4a2-136">Define data factory</span></span>
<span data-ttu-id="9e4a2-137">Objekt pro vytváření dat v šabloně Resource Manager hello definovat, jak je uvedeno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-137">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
<span data-ttu-id="9e4a2-138">Hello dataFactoryName je definováno v "proměnné" jako:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-138">hello dataFactoryName is defined in “variables” as:</span></span>

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a><span data-ttu-id="9e4a2-139">Definování propojených služeb</span><span class="sxs-lookup"><span data-stu-id="9e4a2-139">Define linked services</span></span>

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

<span data-ttu-id="9e4a2-140">V tématu [propojená služba úložiště](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [propojené výpočetní služby](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) podrobné informace o vlastnostech JSON hello hello konkrétní propojené služby chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-140">See [Storage Linked Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [Compute Linked Services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details about hello JSON properties for hello specific linked service you wish toodeploy.</span></span> <span data-ttu-id="9e4a2-141">parametr "dependsOn" Hello Určuje název objektu pro vytváření dat odpovídající hello.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-141">hello “dependsOn” parameter specifies name of hello corresponding data factory.</span></span> <span data-ttu-id="9e4a2-142">Příklad definování propojené služby pro Azure Storage je uveden v hello následující definici JSON:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-142">An example of defining a linked service for Azure Storage is shown in hello following JSON definition:</span></span>

### <a name="define-datasets"></a><span data-ttu-id="9e4a2-143">Definování datové sady</span><span class="sxs-lookup"><span data-stu-id="9e4a2-143">Define datasets</span></span>

```JSON
"type": "datasets",
"name": "[variables('<myDatasetName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<myDatasetLinkedServiceName>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    ...
}
```
<span data-ttu-id="9e4a2-144">Odkazovat příliš[podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) podrobné informace o vlastnostech JSON hello pro typ konkrétní sady hello chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-144">Refer too[Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for details about hello JSON properties for hello specific dataset type you wish toodeploy.</span></span> <span data-ttu-id="9e4a2-145">Poznámka: hello "dependsOn" parametr určuje název odpovídajících dat hello tovární nastavení a úložiště propojené služby.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-145">Note hello “dependsOn” parameter specifies name of hello corresponding data factory and storage linked service.</span></span> <span data-ttu-id="9e4a2-146">Příklad definování typu datovou sadu Azure blob Storage je uveden v hello následující definici JSON:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-146">An example of defining dataset type of Azure blob storage is shown in hello following JSON definition:</span></span>

```JSON
"type": "datasets",
"name": "[variables('storageDataset')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('storageLinkedServiceName')]"
],
"apiVersion": "2015-10-01",
"properties": {
"type": "AzureBlob",
"linkedServiceName": "[variables('storageLinkedServiceName')]",
"typeProperties": {
    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
    "fileName": "[parameters('sourceBlobName')]",
    "format": {
        "type": "TextFormat"
    }
},
"availability": {
    "frequency": "Hour",
    "interval": 1
}
```

### <a name="define-pipelines"></a><span data-ttu-id="9e4a2-147">Definovat kanály</span><span class="sxs-lookup"><span data-stu-id="9e4a2-147">Define pipelines</span></span>

```JSON
"type": "dataPipelines",
"name": "[variables('<mypipelineName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<inputDatasetLinkedServiceName>')]",
    "[variables('<outputDatasetLinkedServiceName>')]",
    "[variables('<inputDataset>')]",
    "[variables('<outputDataset>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    activities: {
        ...
    }
}
```

<span data-ttu-id="9e4a2-148">Odkazovat příliš[definování kanály](data-factory-create-pipelines.md#pipeline-json) pro informace o vlastnostech JSON hello k definování hello konkrétní kanálu a aktivity chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-148">Refer too[defining pipelines](data-factory-create-pipelines.md#pipeline-json) for details about hello JSON properties for defining hello specific pipeline and activities you wish toodeploy.</span></span> <span data-ttu-id="9e4a2-149">Poznámka: hello "dependsOn" parametr určuje název objektu pro vytváření dat hello a všechny odpovídající propojené služby nebo datové sady.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-149">Note hello “dependsOn” parameter specifies name of hello data factory, and any corresponding linked services or datasets.</span></span> <span data-ttu-id="9e4a2-150">V následujícím fragmentu kódu JSON hello je uveden příklad kanálu, který kopíruje data z Azure Blob Storage tooAzure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-150">An example of a pipeline that copies data from Azure Blob Storage tooAzure SQL Database is shown in hello following JSON snippet:</span></span>

```JSON
"type": "datapipelines",
"name": "[variables('pipelineName')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('azureStorageLinkedServiceName')]",
    "[variables('azureSqlLinkedServiceName')]",
    "[variables('blobInputDatasetName')]",
    "[variables('sqlOutputDatasetName')]"
],
"apiVersion": "2015-10-01",
"properties": {
    "activities": [
    {
        "name": "CopyFromAzureBlobToAzureSQL",
        "description": "Copy data frm Azure blob tooAzure SQL",
        "type": "Copy",
        "inputs": [
            {
                "name": "[variables('blobInputDatasetName')]"
            }
        ],
        "outputs": [
            {
                "name": "[variables('sqlOutputDatasetName')]"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
            },
            "translator": {
                "type": "TabularTranslator",
                "columnMappings": "Column0:FirstName,Column1:LastName"
            }
        },
        "Policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 3,
            "timeout": "01:00:00"
        }
    }
    ],
    "start": "2016-10-03T00:00:00Z",
    "end": "2016-10-04T00:00:00Z"
}
```
## <a name="parameterizing-data-factory-template"></a><span data-ttu-id="9e4a2-151">Parametrizace šablony služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="9e4a2-151">Parameterizing Data Factory template</span></span>
<span data-ttu-id="9e4a2-152">Osvědčené postupy v Parametrizace, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) článku.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-152">For best practices on parameterizing, see [Best practices for creating Azure Resource Manager templates](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) article.</span></span> <span data-ttu-id="9e4a2-153">Obecně platí, použití parametrů by měl být minimalizován, zejména v případě, že proměnné lze použít místo.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-153">In general, parameter usage should be minimized, especially if variables can be used instead.</span></span> <span data-ttu-id="9e4a2-154">Jenom zadejte parametry v hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-154">Only provide parameters in hello following scenarios:</span></span>

* <span data-ttu-id="9e4a2-155">Nastavení se liší podle prostředí (Příklad: vývoj, testování a provozním)</span><span class="sxs-lookup"><span data-stu-id="9e4a2-155">Settings vary by environment (example: development, test, and production)</span></span>
* <span data-ttu-id="9e4a2-156">Tajné klíče (jako jsou hesla)</span><span class="sxs-lookup"><span data-stu-id="9e4a2-156">Secrets (such as passwords)</span></span>

<span data-ttu-id="9e4a2-157">Pokud potřebujete toopull tajné klíče z [Azure Key Vault](../key-vault/key-vault-get-started.md) při nasazení entit služby Azure Data Factory pomocí šablony, zadejte hello **trezoru klíčů** a **tajný název** jak je znázorněno v Hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9e4a2-157">If you need toopull secrets from [Azure Key Vault](../key-vault/key-vault-get-started.md) when deploying Azure Data Factory entities using templates, specify hello **key vault** and **secret name** as shown in hello following example:</span></span>

```JSON
"parameters": {
    "storageAccountKey": {
        "reference": {
            "keyVault": {
                "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
             },
            "secretName": "<secretName>"
           },
       },
       ...
}
```

> [!NOTE]
> <span data-ttu-id="9e4a2-158">Při exportu šablony pro existující datové továrny není aktuálně ještě podporovaná, je hello funguje.</span><span class="sxs-lookup"><span data-stu-id="9e4a2-158">While exporting templates for existing data factories is currently not yet supported, it is in hello works.</span></span>
>
>
