---
title: "Pomocí Správce prostředků šablony v datové továrně | Microsoft Docs"
description: "Informace o vytváření a používání šablon Azure Resource Manageru k vytvoření entit služby Data Factory."
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
ms.openlocfilehash: c3ea2c047434b5b5495f0ce85be9376a502e4962
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-templates-to-create-azure-data-factory-entities"></a><span data-ttu-id="deb43-103">Pomocí šablony můžete vytvořit entit služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="deb43-103">Use templates to create Azure Data Factory entities</span></span>
## <a name="overview"></a><span data-ttu-id="deb43-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="deb43-104">Overview</span></span>
<span data-ttu-id="deb43-105">Při použití Azure Data Factory pro integraci vašim datovým potřebám sami zjistíte opakovaného použití stejného vzoru v různých prostředích nebo implementace pro stejnou úlohu opakovaně v rámci stejného řešení.</span><span class="sxs-lookup"><span data-stu-id="deb43-105">While using Azure Data Factory for your data integration needs, you may find yourself reusing the same pattern across different environments or implementing the same task repetitively within the same solution.</span></span> <span data-ttu-id="deb43-106">Šablony vám pomohou implementovat a spravovat tyto scénáře snadno způsobem.</span><span class="sxs-lookup"><span data-stu-id="deb43-106">Templates help you implement and manage these scenarios in an easy manner.</span></span> <span data-ttu-id="deb43-107">Šablony v Azure Data Factory jsou ideální pro scénáře, které zahrnují – opětovné použití a opakování.</span><span class="sxs-lookup"><span data-stu-id="deb43-107">Templates in Azure Data Factory are ideal for scenarios that involve reusability and repetition.</span></span>

<span data-ttu-id="deb43-108">Vezměte v úvahu situaci, kde může organizace má 10 výrobních závodech po celém světě.</span><span class="sxs-lookup"><span data-stu-id="deb43-108">Consider the situation where an organization has 10 manufacturing plants across the world.</span></span> <span data-ttu-id="deb43-109">Protokoly z každého zařízení jsou uloženy v databázi systému SQL Server samostatný místní.</span><span class="sxs-lookup"><span data-stu-id="deb43-109">The logs from each plant are stored in a separate on-premises SQL Server database.</span></span> <span data-ttu-id="deb43-110">Společnost chce sestavení jednoho datového skladu v cloudu pro ad-hoc analýzu.</span><span class="sxs-lookup"><span data-stu-id="deb43-110">The company wants to build a single data warehouse in the cloud for ad-hoc analytics.</span></span> <span data-ttu-id="deb43-111">Také chce mít stejné logiky, která ale různé konfigurace pro vývoj, testování a provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="deb43-111">It also wants to have the same logic but different configurations for development, test, and production environments.</span></span>

<span data-ttu-id="deb43-112">V takovém případě musí opakovat ve stejném prostředí, ale s různými hodnotami mezi 10 datové továrny pro každého z výrobních závodů úlohu.</span><span class="sxs-lookup"><span data-stu-id="deb43-112">In this case, a task needs to be repeated within the same environment, but with different values across the 10 data factories for each manufacturing plant.</span></span> <span data-ttu-id="deb43-113">Ve skutečnosti **opakování** je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="deb43-113">In effect, **repetition** is present.</span></span> <span data-ttu-id="deb43-114">Ukázka umožňuje abstrakce tento obecný tok (to znamená, že stejné aktivity v každé služby data factory kanály), ale používá soubor samostatného parametru pro každého z výrobních závodů.</span><span class="sxs-lookup"><span data-stu-id="deb43-114">Templating allows the abstraction of this generic flow (that is, pipelines having the same activities in each data factory), but uses a separate parameter file for each manufacturing plant.</span></span>

<span data-ttu-id="deb43-115">Kromě toho, jak organizace chce nasadit tyto 10 datové továrny vícekrát různých prostředích, šablony můžete použít **– opětovné použití** s využitím soubory samostatného parametru pro vývoj, testování a provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="deb43-115">Furthermore, as the organization wants to deploy these 10 data factories multiple times across different environments, templates can use this **reusability** by utilizing separate parameter files for development, test, and production environments.</span></span>

## <a name="templating-with-azure-resource-manager"></a><span data-ttu-id="deb43-116">Ukázka s Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="deb43-116">Templating with Azure Resource Manager</span></span>
<span data-ttu-id="deb43-117">[Šablony Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#template-deployment) jsou skvělý způsob, jak dosáhnout ukázka v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="deb43-117">[Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md#template-deployment) are a great way to achieve templating in Azure Data Factory.</span></span> <span data-ttu-id="deb43-118">Šablony Resource Manageru definovat infrastrukturu a konfiguraci řešení Azure prostřednictvím soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="deb43-118">Resource Manager templates define the infrastructure and configuration of your Azure solution through a JSON file.</span></span> <span data-ttu-id="deb43-119">Protože šablon Azure Resource Manageru pracovat se službami Azure všechny nebo většinu, je široce umožňuje snadno spravovat všechny prostředky vaše prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="deb43-119">Because Azure Resource Manager templates work with all/most Azure services, it can be widely used to easily manage all resources of your Azure assets.</span></span> <span data-ttu-id="deb43-120">V tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) Další informace o šablonách Resource Manager obecně.</span><span class="sxs-lookup"><span data-stu-id="deb43-120">See [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) to learn more about the Resource Manager Templates in general.</span></span>

## <a name="tutorials"></a><span data-ttu-id="deb43-121">Kurzy</span><span class="sxs-lookup"><span data-stu-id="deb43-121">Tutorials</span></span>
<span data-ttu-id="deb43-122">Najdete v následujících kurzech podrobné pokyny k vytvoření entit služby Data Factory pomocí šablony Resource Manageru:</span><span class="sxs-lookup"><span data-stu-id="deb43-122">See the following tutorials for step-by-step instructions to create Data Factory entities by using Resource Manager templates:</span></span>

* [<span data-ttu-id="deb43-123">Kurz: Vytvoření kanálu ke zkopírování dat pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="deb43-123">Tutorial: Create a pipeline to copy data by using Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="deb43-124">Kurz: Vytvoření kanálu pro zpracování dat pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="deb43-124">Tutorial: Create a pipeline to process data by using Azure Resource Manager template</span></span>](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a><span data-ttu-id="deb43-125">Data Factory šablony na Githubu</span><span class="sxs-lookup"><span data-stu-id="deb43-125">Data Factory templates on GitHub</span></span>
<span data-ttu-id="deb43-126">Podívejte se na následující šablony Azure rychlý start na Githubu:</span><span class="sxs-lookup"><span data-stu-id="deb43-126">Check out the following Azure quick start templates on GitHub:</span></span>

* [<span data-ttu-id="deb43-127">Vytvoření Data factory ke zkopírování dat z Azure Blob Storage do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="deb43-127">Create a Data factory to copy data from Azure Blob Storage to Azure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [<span data-ttu-id="deb43-128">Vytvoření služby Data factory s aktivitou Hive v clusteru Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="deb43-128">Create a Data factory with Hive activity on Azure HDInsight cluster</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [<span data-ttu-id="deb43-129">Vytvoření Data factory ke zkopírování dat ze služby Salesforce do objektů BLOB služby Azure</span><span class="sxs-lookup"><span data-stu-id="deb43-129">Create a Data factory to copy data from Salesforce to Azure Blobs</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [<span data-ttu-id="deb43-130">Vytvoření objektu pro vytváření dat, který je zřetězený aktivity: kopíruje data ze serveru FTP do objektů BLOB služby Azure, vyvolá skriptu hive v clusteru HDInsight na vyžádání pro transformaci dat a zkopíruje výsledek do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="deb43-130">Create a Data factory that chains activities: copies data from an FTP server to Azure Blobs, invokes a hive script on an on-demand HDInsight cluster to transform the data, and copies result into Azure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

<span data-ttu-id="deb43-131">Nebojte se sdílet své šablony Azure Data Factory v [Azure rychlý start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="deb43-131">Feel free to share your Azure Data Factory templates at [Azure Quick start](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="deb43-132">Odkazovat [příspěvku průvodce](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) při vývoji šablony, které je možné sdílet přes toto úložiště.</span><span class="sxs-lookup"><span data-stu-id="deb43-132">Refer to the [contribution guide](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) while developing templates that can be shared via this repository.</span></span>

<span data-ttu-id="deb43-133">Následující části obsahují podrobnosti o definice prostředků pro vytváření dat v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="deb43-133">The following sections provide details about defining Data Factory resources in a Resource Manager template.</span></span>

## <a name="defining-data-factory-resources-in-templates"></a><span data-ttu-id="deb43-134">Definice prostředků pro vytváření dat v šablonách</span><span class="sxs-lookup"><span data-stu-id="deb43-134">Defining Data Factory resources in templates</span></span>
<span data-ttu-id="deb43-135">Je nejvyšší úrovně šablonu pro definování objekt pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="deb43-135">The top-level template for defining a data factory is:</span></span>

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

### <a name="define-data-factory"></a><span data-ttu-id="deb43-136">Definování datové továrny</span><span class="sxs-lookup"><span data-stu-id="deb43-136">Define data factory</span></span>
<span data-ttu-id="deb43-137">Datovou továrnu definujete v šabloně Resource Manageru, jak je znázorněno v následující ukázce:</span><span class="sxs-lookup"><span data-stu-id="deb43-137">You define a data factory in the Resource Manager template as shown in the following sample:</span></span>

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
<span data-ttu-id="deb43-138">DataFactoryName je definováno v "proměnné" jako:</span><span class="sxs-lookup"><span data-stu-id="deb43-138">The dataFactoryName is defined in “variables” as:</span></span>

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a><span data-ttu-id="deb43-139">Definování propojených služeb</span><span class="sxs-lookup"><span data-stu-id="deb43-139">Define linked services</span></span>

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

<span data-ttu-id="deb43-140">V tématu [propojená služba úložiště](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [propojené výpočetní služby](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) podrobnosti o vlastnostech JSON pro konkrétní propojené služby, které chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="deb43-140">See [Storage Linked Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [Compute Linked Services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details about the JSON properties for the specific linked service you wish to deploy.</span></span> <span data-ttu-id="deb43-141">Parametr "dependsOn" Určuje název objektu pro vytváření dat odpovídající.</span><span class="sxs-lookup"><span data-stu-id="deb43-141">The “dependsOn” parameter specifies name of the corresponding data factory.</span></span> <span data-ttu-id="deb43-142">V následující definici JSON je znázorněn příklad definování propojené služby pro Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="deb43-142">An example of defining a linked service for Azure Storage is shown in the following JSON definition:</span></span>

### <a name="define-datasets"></a><span data-ttu-id="deb43-143">Definování datové sady</span><span class="sxs-lookup"><span data-stu-id="deb43-143">Define datasets</span></span>

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
<span data-ttu-id="deb43-144">Odkazovat na [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) podrobné informace o vlastnostech JSON pro typ konkrétní datové sady, které chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="deb43-144">Refer to [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for details about the JSON properties for the specific dataset type you wish to deploy.</span></span> <span data-ttu-id="deb43-145">Všimněte si, že parametr "dependsOn" Určuje název odpovídající objekt pro vytváření dat a úložiště propojená služba.</span><span class="sxs-lookup"><span data-stu-id="deb43-145">Note the “dependsOn” parameter specifies name of the corresponding data factory and storage linked service.</span></span> <span data-ttu-id="deb43-146">V následující definici JSON je uveden příklad definování typu datovou sadu Azure blob Storage:</span><span class="sxs-lookup"><span data-stu-id="deb43-146">An example of defining dataset type of Azure blob storage is shown in the following JSON definition:</span></span>

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

### <a name="define-pipelines"></a><span data-ttu-id="deb43-147">Definovat kanály</span><span class="sxs-lookup"><span data-stu-id="deb43-147">Define pipelines</span></span>

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

<span data-ttu-id="deb43-148">Odkazovat na [definování kanály](data-factory-create-pipelines.md#pipeline-json) podrobné informace o vlastnostech JSON pro definování konkrétní kanálu a aktivity, které chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="deb43-148">Refer to [defining pipelines](data-factory-create-pipelines.md#pipeline-json) for details about the JSON properties for defining the specific pipeline and activities you wish to deploy.</span></span> <span data-ttu-id="deb43-149">Poznámka: parametr "dependsOn" Určuje název objektu pro vytváření dat a všechny odpovídající propojené služby nebo datové sady.</span><span class="sxs-lookup"><span data-stu-id="deb43-149">Note the “dependsOn” parameter specifies name of the data factory, and any corresponding linked services or datasets.</span></span> <span data-ttu-id="deb43-150">Příklad kanálu, který kopíruje data z Azure Blob Storage do Azure SQL Database je uveden v následujícím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="deb43-150">An example of a pipeline that copies data from Azure Blob Storage to Azure SQL Database is shown in the following JSON snippet:</span></span>

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
        "description": "Copy data frm Azure blob to Azure SQL",
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
## <a name="parameterizing-data-factory-template"></a><span data-ttu-id="deb43-151">Parametrizace šablony služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="deb43-151">Parameterizing Data Factory template</span></span>
<span data-ttu-id="deb43-152">Osvědčené postupy v Parametrizace, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) článku.</span><span class="sxs-lookup"><span data-stu-id="deb43-152">For best practices on parameterizing, see [Best practices for creating Azure Resource Manager templates](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) article.</span></span> <span data-ttu-id="deb43-153">Obecně platí, použití parametrů by měl být minimalizován, zejména v případě, že proměnné lze použít místo.</span><span class="sxs-lookup"><span data-stu-id="deb43-153">In general, parameter usage should be minimized, especially if variables can be used instead.</span></span> <span data-ttu-id="deb43-154">Jenom zadejte parametry v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="deb43-154">Only provide parameters in the following scenarios:</span></span>

* <span data-ttu-id="deb43-155">Nastavení se liší podle prostředí (Příklad: vývoj, testování a provozním)</span><span class="sxs-lookup"><span data-stu-id="deb43-155">Settings vary by environment (example: development, test, and production)</span></span>
* <span data-ttu-id="deb43-156">Tajné klíče (jako jsou hesla)</span><span class="sxs-lookup"><span data-stu-id="deb43-156">Secrets (such as passwords)</span></span>

<span data-ttu-id="deb43-157">Pokud potřebujete vyžádat tajné klíče z [Azure Key Vault](../key-vault/key-vault-get-started.md) při nasazení entit služby Azure Data Factory pomocí šablony, zadejte **trezoru klíčů** a **tajný název** jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="deb43-157">If you need to pull secrets from [Azure Key Vault](../key-vault/key-vault-get-started.md) when deploying Azure Data Factory entities using templates, specify the **key vault** and **secret name** as shown in the following example:</span></span>

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
> <span data-ttu-id="deb43-158">Při exportu šablony pro existující datové továrny není aktuálně ještě podporovaná, je ve službě funguje.</span><span class="sxs-lookup"><span data-stu-id="deb43-158">While exporting templates for existing data factories is currently not yet supported, it is in the works.</span></span>
>
>
