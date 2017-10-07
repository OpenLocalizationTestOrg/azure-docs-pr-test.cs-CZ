---
title: "Kurz: Vytvoření kanálu pomocí šablony Resource Manageru | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory pomocí šablony Azure Resource Manageru. Tento kanál kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1274e11a-e004-4df5-af07-850b2de7c15e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 1c7567cb0423f7ce3e0cab2d77a4d861b70eb56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-azure-resource-manager-template-toocreate-a-data-factory-pipeline-toocopy-data"></a><span data-ttu-id="e39cd-104">Kurz: Správce prostředků Azure pomocí šablony toocreate toocopy datový kanál služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="e39cd-104">Tutorial: Use Azure Resource Manager template toocreate a Data Factory pipeline toocopy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e39cd-105">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="e39cd-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="e39cd-106">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="e39cd-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="e39cd-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e39cd-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="e39cd-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e39cd-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="e39cd-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e39cd-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="e39cd-110">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e39cd-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="e39cd-111">REST API</span><span class="sxs-lookup"><span data-stu-id="e39cd-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="e39cd-112">.NET API</span><span class="sxs-lookup"><span data-stu-id="e39cd-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="e39cd-113">Tento kurz ukazuje, jak toouse toocreate šablony Azure Resource Manager služby Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="e39cd-113">This tutorial shows you how toouse an Azure Resource Manager template toocreate an Azure data factory.</span></span> <span data-ttu-id="e39cd-114">Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="e39cd-114">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="e39cd-115">Ho nebudou transformovat vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="e39cd-115">It does not transform input data tooproduce output data.</span></span> <span data-ttu-id="e39cd-116">Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="e39cd-116">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="e39cd-117">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="e39cd-117">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="e39cd-118">Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="e39cd-118">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="e39cd-119">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="e39cd-119">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="e39cd-120">Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="e39cd-120">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="e39cd-121">Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="e39cd-121">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="e39cd-122">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="e39cd-122">A pipeline can have more than one activity.</span></span> <span data-ttu-id="e39cd-123">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="e39cd-123">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="e39cd-124">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="e39cd-124">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="e39cd-125">Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="e39cd-125">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="e39cd-126">Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="e39cd-126">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e39cd-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e39cd-127">Prerequisites</span></span>
* <span data-ttu-id="e39cd-128">Projděte [přehled kurzu a požadované součásti](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="e39cd-128">Go through [Tutorial Overview and Prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="e39cd-129">Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku tooinstall nejnovější verzi prostředí Azure PowerShell ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e39cd-129">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span> <span data-ttu-id="e39cd-130">V tomto kurzu použijete entit služby Data Factory toodeploy prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e39cd-130">In this tutorial, you use PowerShell toodeploy Data Factory entities.</span></span> 
* <span data-ttu-id="e39cd-131">(volitelné) V tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) toolearn o šablonách Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e39cd-131">(optional) See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn about Azure Resource Manager templates.</span></span>

## <a name="in-this-tutorial"></a><span data-ttu-id="e39cd-132">V tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="e39cd-132">In this tutorial</span></span>
<span data-ttu-id="e39cd-133">V tomto kurzu vytvoříte objekt pro vytváření dat s hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="e39cd-133">In this tutorial, you create a data factory with hello following Data Factory entities:</span></span>

| <span data-ttu-id="e39cd-134">Entita</span><span class="sxs-lookup"><span data-stu-id="e39cd-134">Entity</span></span> | <span data-ttu-id="e39cd-135">Popis</span><span class="sxs-lookup"><span data-stu-id="e39cd-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e39cd-136">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e39cd-136">Azure Storage linked service</span></span> |<span data-ttu-id="e39cd-137">Propojí datovou továrnu toohello účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e39cd-137">Links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="e39cd-138">Azure Storage je hello zdrojového úložiště dat a Azure SQL database je hello podřízený datové úložiště pro aktivitu kopírování hello v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-138">Azure Storage is hello source data store and Azure SQL database is hello sink data store for hello copy activity in hello tutorial.</span></span> <span data-ttu-id="e39cd-139">Určuje účet úložiště hello, který obsahuje hello vstupní data pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-139">It specifies hello storage account that contains hello input data for hello copy activity.</span></span> |
| <span data-ttu-id="e39cd-140">Propojená služba Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e39cd-140">Azure SQL Database linked service</span></span> |<span data-ttu-id="e39cd-141">Propojí toohello datovou továrnu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="e39cd-141">Links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="e39cd-142">Určuje hello Azure SQL database, která obsahuje hello výstupní data pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-142">It specifies hello Azure SQL database that holds hello output data for hello copy activity.</span></span> |
| <span data-ttu-id="e39cd-143">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="e39cd-143">Azure Blob input dataset</span></span> |<span data-ttu-id="e39cd-144">Odkazuje toohello propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e39cd-144">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="e39cd-145">Hello propojená služba odkazuje tooan účtu Azure Storage a datové sady objektu Blob Azure hello určuje hello kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-145">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello input data.</span></span> |
| <span data-ttu-id="e39cd-146">Výstupní datová sada Azure SQL</span><span class="sxs-lookup"><span data-stu-id="e39cd-146">Azure SQL output dataset</span></span> |<span data-ttu-id="e39cd-147">Odkazuje toohello propojená služba Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="e39cd-147">Refers toohello Azure SQL linked service.</span></span> <span data-ttu-id="e39cd-148">Hello propojená služba Azure SQL odkazuje tooan server Azure SQL a Azure SQL hello datovou sadu Určuje název hello hello tabulky, který obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="e39cd-148">hello Azure SQL linked service refers tooan Azure SQL server and hello Azure SQL dataset specifies hello name of hello table that holds hello output data.</span></span> |
| <span data-ttu-id="e39cd-149">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="e39cd-149">Data pipeline</span></span> |<span data-ttu-id="e39cd-150">Hello kanálu má jednu aktivitu zadejte kopírování, která přebírá hello datovou sadu objektů blob v Azure jako vstup a hello Azure SQL datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="e39cd-150">hello pipeline has one activity of type Copy that takes hello Azure blob dataset as an input and hello Azure SQL dataset as an output.</span></span> <span data-ttu-id="e39cd-151">Aktivita kopírování Hello kopíruje data ze objektů blob v Azure tooa tabulky v databázi Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-151">hello copy activity copies data from an Azure blob tooa table in hello Azure SQL database.</span></span> |

<span data-ttu-id="e39cd-152">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="e39cd-152">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="e39cd-153">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="e39cd-153">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="e39cd-154">Existují dva typy aktivit: [aktivity přesunu dat](data-factory-data-movement-activities.md) a [transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="e39cd-154">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="e39cd-155">V tomto kurzu vytvoříte kanál s jednou aktivitou (aktivita kopírování).</span><span class="sxs-lookup"><span data-stu-id="e39cd-155">In this tutorial, you create a pipeline with one activity (copy activity).</span></span>

![Zkopírujte tooAzure objektů Blob v Azure SQL Database](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

<span data-ttu-id="e39cd-157">Hello následující část obsahuje hello dokončení šablony Resource Manageru pro definování entit služby Data Factory, takže můžete rychle spustit prostřednictvím hello kurzu a testování hello šablony.</span><span class="sxs-lookup"><span data-stu-id="e39cd-157">hello following section provides hello complete Resource Manager template for defining Data Factory entities so that you can quickly run through hello tutorial and test hello template.</span></span> <span data-ttu-id="e39cd-158">toounderstand o každé entity služby Data Factory je definován, v tématu [entit služby Data Factory v šabloně hello](#data-factory-entities-in-the-template) části.</span><span class="sxs-lookup"><span data-stu-id="e39cd-158">toounderstand how each Data Factory entity is defined, see [Data Factory entities in hello template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="e39cd-159">Šablona JSON služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="e39cd-159">Data Factory JSON template</span></span>
<span data-ttu-id="e39cd-160">Hello nejvyšší úrovně šablony Resource Manageru pro definování objekt pro vytváření dat je:</span><span class="sxs-lookup"><span data-stu-id="e39cd-160">hello top-level Resource Manager template for defining a data factory is:</span></span> 

```json
{
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
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```
<span data-ttu-id="e39cd-161">Vytvořte soubor JSON s názvem **ADFCopyTutorialARM.json** v **C:\ADFGetStarted** složku s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="e39cd-161">Create a JSON file named **ADFCopyTutorialARM.json** in **C:\ADFGetStarted** folder with hello following content:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
      "storageAccountName": { "type": "string", "metadata": { "description": "Name of hello Azure storage account that contains hello data toobe copied." } },
      "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for hello Azure storage account." } },
      "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of hello blob container in hello Azure Storage account." } },
      "sourceBlobName": { "type": "string", "metadata": { "description": "Name of hello blob in hello container that has hello data toobe copied tooAzure SQL Database table" } },
      "sqlServerName": { "type": "string", "metadata": { "description": "Name of hello Azure SQL Server that will hold hello output/copied data." } },
      "databaseName": { "type": "string", "metadata": { "description": "Name of hello Azure SQL Database in hello Azure SQL server." } },
      "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of hello user that has access toohello Azure SQL server." } },
      "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for hello user." } },
      "targetSQLTable": { "type": "string", "metadata": { "description": "Table in hello Azure SQL Database that will hold hello copied data." } 
      } 
    },
    "variables": {
      "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
      "azureSqlLinkedServiceName": "AzureSqlLinkedService",
      "azureStorageLinkedServiceName": "AzureStorageLinkedService",
      "blobInputDatasetName": "BlobInputDataset",
      "sqlOutputDatasetName": "SQLOutputDataset",
      "pipelineName": "Blob2SQLPipeline"
    },
    "resources": [
      {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US",
        "resources": [
          {
            "type": "linkedservices",
            "name": "[variables('azureStorageLinkedServiceName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureStorage",
              "description": "Azure Storage linked service",
              "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
              }
            }
          },
          {
            "type": "linkedservices",
            "name": "[variables('azureSqlLinkedServiceName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlDatabase",
              "description": "Azure SQL linked service",
              "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
              }
            }
          },
          {
            "type": "datasets",
            "name": "[variables('blobInputDatasetName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]",
              "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureBlob",
              "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
              "structure": [
                {
                  "name": "Column0",
                  "type": "String"
                },
                {
                  "name": "Column1",
                  "type": "String"
                }
              ],
              "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
                }
              },
              "availability": {
                "frequency": "Hour",
                "interval": 1
              },
              "external": true
            }
          },
          {
            "type": "datasets",
            "name": "[variables('sqlOutputDatasetName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]",
              "[variables('azureSqlLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlTable",
              "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
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
              "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
              },
              "availability": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          {
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
              "start": "2017-05-11T00:00:00Z",
              "end": "2017-05-12T00:00:00Z"
            }
          }
        ]
      }
    ]
  }
```

## <a name="parameters-json"></a><span data-ttu-id="e39cd-162">Parametry JSON</span><span class="sxs-lookup"><span data-stu-id="e39cd-162">Parameters JSON</span></span>
<span data-ttu-id="e39cd-163">Vytvořte soubor JSON s názvem **ADFCopyTutorialARM Parameters.JSON tímto kódem** obsahující parametry šablony Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-163">Create a JSON file named **ADFCopyTutorialARM-Parameters.json** that contains parameters for hello Azure Resource Manager template.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="e39cd-164">Zadejte název a klíč svého účtu služby Azure Storage v parametrech storageAccountName a storageAccountKey.</span><span class="sxs-lookup"><span data-stu-id="e39cd-164">Specify name and key of your Azure Storage account for storageAccountName and storageAccountKey parameters.</span></span>  
> 
> <span data-ttu-id="e39cd-165">Zadejte server SQL Azure, databázi, uživatele a heslo v parametrech sqlServerName, databaseName, sqlServerUserName, a sqlServerPassword.</span><span class="sxs-lookup"><span data-stu-id="e39cd-165">Specify Azure SQL server, database, user, and password for sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters.</span></span>  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "storageAccountName": { "value": "<Name of hello Azure storage account>"    },
        "storageAccountKey": {
            "value": "<Key for hello Azure storage account>"
        },
        "sourceBlobContainer": { "value": "adftutorial" },
        "sourceBlobName": { "value": "emp.txt" },
        "sqlServerName": { "value": "<Name of hello Azure SQL server>" },
        "databaseName": { "value": "<Name of hello Azure SQL database>" },
        "sqlServerUserName": { "value": "<Name of hello user who has access toohello Azure SQL database>" },
        "sqlServerPassword": { "value": "<password for hello user>" },
        "targetSQLTable": { "value": "emp" }
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="e39cd-166">Může mít soubory JSON samostatného parametru pro vývoj, testování a provozním prostředí, která můžete použít s hello stejné šablony Data Factory JSON.</span><span class="sxs-lookup"><span data-stu-id="e39cd-166">You may have separate parameter JSON files for development, testing, and production environments that you can use with hello same Data Factory JSON template.</span></span> <span data-ttu-id="e39cd-167">Pomocí skriptu prostředí PowerShell můžete zautomatizovat nasazování entit služby Data Factory v těchto prostředích.</span><span class="sxs-lookup"><span data-stu-id="e39cd-167">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span>  
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="e39cd-168">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="e39cd-168">Create data factory</span></span>
1. <span data-ttu-id="e39cd-169">Spustit **prostředí Azure PowerShell** a hello spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e39cd-169">Start **Azure PowerShell** and run hello following command:</span></span>
   * <span data-ttu-id="e39cd-170">Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e39cd-170">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount    
    ```  
   * <span data-ttu-id="e39cd-171">Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="e39cd-171">Run hello following command tooview all hello subscriptions for this account.</span></span>
   
    ```PowerShell
    Get-AzureRmSubscription
    ```   
   * <span data-ttu-id="e39cd-172">Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-172">Run hello following command tooselect hello subscription that you want toowork with.</span></span>
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. <span data-ttu-id="e39cd-173">Spusťte hello následující entity služby Data Factory toodeploy příkaz pomocí šablony Resource Manageru hello, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="e39cd-173">Run hello following command toodeploy Data Factory entities using hello Resource Manager template you created in Step 1.</span></span>

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="e39cd-174">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="e39cd-174">Monitor pipeline</span></span>

1. <span data-ttu-id="e39cd-175">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="e39cd-175">Log in toohello [Azure portal](https://portal.azure.com) using your Azure account.</span></span>
2. <span data-ttu-id="e39cd-176">Klikněte na tlačítko **datové továrny** v hello levé nabídce (nebo) klikněte na tlačítko **další služby** a klikněte na tlačítko **datové továrny** pod **INTELLIGENCE + analýzy** kategorie.</span><span class="sxs-lookup"><span data-stu-id="e39cd-176">Click **Data factories** on hello left menu (or) click **More services** and click **Data factories** under **INTELLIGENCE + ANALYTICS** category.</span></span>
   
    ![Nabídka Datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. <span data-ttu-id="e39cd-178">V hello **datové továrny** stránky, Hledat a najít služby data factory (AzureBlobToAzureSQLDatabaseDF).</span><span class="sxs-lookup"><span data-stu-id="e39cd-178">In hello **Data factories** page, search for and find your data factory (AzureBlobToAzureSQLDatabaseDF).</span></span> 
   
    ![Vyhledávání datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. <span data-ttu-id="e39cd-180">Klikněte na svou datovou továrnu Azure.</span><span class="sxs-lookup"><span data-stu-id="e39cd-180">Click your Azure data factory.</span></span> <span data-ttu-id="e39cd-181">Zobrazí hello Domovská stránka objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-181">You see hello home page for hello data factory.</span></span>
   
    ![Domovská stránka datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. <span data-ttu-id="e39cd-183">Postupujte podle pokynů v [monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello kanálu a datových sad v tomto kurzu jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e39cd-183">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="e39cd-184">V současné době Visual Studio monitorování kanálů Data Factory nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="e39cd-184">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span>
7. <span data-ttu-id="e39cd-185">Pokud je řez v hello **připraven** stavu, ověřte, že hello data zkopírovaný toohello **emp** tabulky v databázi Azure SQL hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-185">When a slice is in hello **Ready** state, verify that hello data is copied toohello **emp** table in hello Azure SQL database.</span></span>


<span data-ttu-id="e39cd-186">Další informace o tom, jak toouse oken Azure portal toomonitor kanálu a datových sad jste vytvořili v tomto kurzu, najdete v části [monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md) .</span><span class="sxs-lookup"><span data-stu-id="e39cd-186">For more information on how toouse Azure portal blades toomonitor pipeline and datasets you have created in this tutorial, see [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) .</span></span>

<span data-ttu-id="e39cd-187">Další informace o tom, jak toouse hello monitorování a Správa aplikací toomonitor data kanálů najdete v tématu [monitorování a Správa kanálů služby Azure Data Factory pomocí monitorovací aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="e39cd-187">For more information on how toouse hello Monitor & Manage application toomonitor your data pipelines, see [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md).</span></span>

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="e39cd-188">Entity objektu pro vytváření dat v šabloně hello</span><span class="sxs-lookup"><span data-stu-id="e39cd-188">Data Factory entities in hello template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="e39cd-189">Definování datové továrny</span><span class="sxs-lookup"><span data-stu-id="e39cd-189">Define data factory</span></span>
<span data-ttu-id="e39cd-190">Objekt pro vytváření dat v šabloně Resource Manager hello definovat, jak je uvedeno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="e39cd-190">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```

<span data-ttu-id="e39cd-191">Hello dataFactoryName je definován jako:</span><span class="sxs-lookup"><span data-stu-id="e39cd-191">hello dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

<span data-ttu-id="e39cd-192">Jedná se o jedinečný řetězec na základě ID hello prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="e39cd-192">It is a unique string based on hello resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="e39cd-193">Definování entit služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="e39cd-193">Defining Data Factory entities</span></span>
<span data-ttu-id="e39cd-194">Hello následující entity služby Data Factory jsou definovány v šabloně hello JSON:</span><span class="sxs-lookup"><span data-stu-id="e39cd-194">hello following Data Factory entities are defined in hello JSON template:</span></span> 

1. [<span data-ttu-id="e39cd-195">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e39cd-195">Azure Storage linked service</span></span>](#azure-storage-linked-service)
2. [<span data-ttu-id="e39cd-196">Propojená služba Azure SQL</span><span class="sxs-lookup"><span data-stu-id="e39cd-196">Azure SQL linked service</span></span>](#azure-sql-database-linked-service)
3. [<span data-ttu-id="e39cd-197">Datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="e39cd-197">Azure blob dataset</span></span>](#azure-blob-dataset)
4. [<span data-ttu-id="e39cd-198">Datová sada Azure SQL</span><span class="sxs-lookup"><span data-stu-id="e39cd-198">Azure SQL dataset</span></span>](#azure-sql-dataset)
5. [<span data-ttu-id="e39cd-199">Data Pipeline s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="e39cd-199">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="e39cd-200">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e39cd-200">Azure Storage linked service</span></span>
<span data-ttu-id="e39cd-201">Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="e39cd-201">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="e39cd-202">Vytvořit kontejner a účet úložiště dat toothis odesílané jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="e39cd-202">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="e39cd-203">Zadejte název hello a klíč účtu úložiště Azure v této části.</span><span class="sxs-lookup"><span data-stu-id="e39cd-203">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="e39cd-204">V tématu [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="e39cd-204">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span> 

```json
{
    "type": "linkedservices",
    "name": "[variables('azureStorageLinkedServiceName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureStorage",
        "description": "Azure Storage linked service",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```

<span data-ttu-id="e39cd-205">Hello connectionString používá hello storageAccountName a storageAccountKey parametry.</span><span class="sxs-lookup"><span data-stu-id="e39cd-205">hello connectionString uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="e39cd-206">Hello hodnoty pro tyto parametry předané pomocí konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="e39cd-206">hello values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="e39cd-207">definice Hello také používá proměnné: azureStroageLinkedService a dataFactoryName definovaná v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-207">hello definition also uses variables: azureStroageLinkedService and dataFactoryName defined in hello template.</span></span> 

#### <a name="azure-sql-database-linked-service"></a><span data-ttu-id="e39cd-208">Propojená služba Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e39cd-208">Azure SQL Database linked service</span></span>
<span data-ttu-id="e39cd-209">AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="e39cd-209">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="e39cd-210">Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi.</span><span class="sxs-lookup"><span data-stu-id="e39cd-210">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="e39cd-211">Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="e39cd-211">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="e39cd-212">Zadejte název serveru Azure SQL hello, název databáze, uživatelské jméno a heslo uživatele v této části.</span><span class="sxs-lookup"><span data-stu-id="e39cd-212">You specify hello Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="e39cd-213">V tématu [propojená služba Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) podrobnosti o toodefine vlastnosti používané JSON Azure SQL propojené služby.</span><span class="sxs-lookup"><span data-stu-id="e39cd-213">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used toodefine an Azure SQL linked service.</span></span>  

```json
{
    "type": "linkedservices",
    "name": "[variables('azureSqlLinkedServiceName')]",
    "dependsOn": [
      "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlDatabase",
          "description": "Azure SQL linked service",
          "typeProperties": {
            "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
          }
    }
}
```

<span data-ttu-id="e39cd-214">Hello connectionString používá sqlServerName, databaseName, sqlServerUserName a sqlServerPassword parametry, jejichž hodnoty se předávají pomocí konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="e39cd-214">hello connectionString uses sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters whose values are passed by using a configuration file.</span></span> <span data-ttu-id="e39cd-215">Hello definice také používá následující proměnné z šablony hello hello: azureSqlLinkedServiceName dataFactoryName.</span><span class="sxs-lookup"><span data-stu-id="e39cd-215">hello definition also uses hello following variables from hello template: azureSqlLinkedServiceName, dataFactoryName.</span></span>

#### <a name="azure-blob-dataset"></a><span data-ttu-id="e39cd-216">Datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="e39cd-216">Azure blob dataset</span></span>
<span data-ttu-id="e39cd-217">Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e39cd-217">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="e39cd-218">V definici datové sady objektu blob Azure zadejte názvy kontejneru objektů blob, složku a soubor, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-218">In Azure blob dataset definition, you specify names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="e39cd-219">V tématu [vlastnosti datové sady objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="e39cd-219">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span> 

```json
{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
      "[variables('dataFactoryName')]",
      "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
        "structure": [
        {
              "name": "Column0",
              "type": "String"
        },
        {
              "name": "Column1",
              "type": "String"
        }
          ],
          "typeProperties": {
            "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
            "fileName": "[parameters('sourceBlobName')]",
            "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Hour",
            "interval": 1
          },
          "external": true
    }
}
```

#### <a name="azure-sql-dataset"></a><span data-ttu-id="e39cd-220">Datová sada Azure SQL</span><span class="sxs-lookup"><span data-stu-id="e39cd-220">Azure SQL dataset</span></span>
<span data-ttu-id="e39cd-221">Zadáte název hello hello tabulky v databázi Azure SQL hello, která obsahuje hello zkopírovat data z hello úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="e39cd-221">You specify hello name of hello table in hello Azure SQL database that holds hello copied data from hello Azure Blob storage.</span></span> <span data-ttu-id="e39cd-222">V tématu [vlastnosti datové sady Azure SQL](data-factory-azure-sql-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="e39cd-222">See [Azure SQL dataset properties](data-factory-azure-sql-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure SQL dataset.</span></span> 

```json
{
    "type": "datasets",
    "name": "[variables('sqlOutputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
          "[variables('azureSqlLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlTable",
          "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
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
          "typeProperties": {
            "tableName": "[parameters('targetSQLTable')]"
          },
          "availability": {
            "frequency": "Hour",
            "interval": 1
          }
    }
}
```

#### <a name="data-pipeline"></a><span data-ttu-id="e39cd-223">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="e39cd-223">Data pipeline</span></span>
<span data-ttu-id="e39cd-224">Můžete definovat kanál, který kopíruje data z datové sady hello objektů blob v Azure datovou sadu toohello Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="e39cd-224">You define a pipeline that copies data from hello Azure blob dataset toohello Azure SQL dataset.</span></span> <span data-ttu-id="e39cd-225">V tématu [JSON kanálu](data-factory-create-pipelines.md#pipeline-json) popisy toodefine prvky používané JSON kanálu v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="e39cd-225">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span> 

```json
{
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
          "start": "2017-05-11T00:00:00Z",
          "end": "2017-05-12T00:00:00Z"
    }
}
```

## <a name="reuse-hello-template"></a><span data-ttu-id="e39cd-226">Opakované použití hello šablony</span><span class="sxs-lookup"><span data-stu-id="e39cd-226">Reuse hello template</span></span>
<span data-ttu-id="e39cd-227">V kurzu hello jste vytvořili šablonu pro definování entit služby Data Factory a šablonu pro předávání hodnot pro parametry.</span><span class="sxs-lookup"><span data-stu-id="e39cd-227">In hello tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="e39cd-228">Hello kanál kopíruje data z databáze Azure SQL tooan účtu Azure Storage zadaný prostřednictvím parametrů.</span><span class="sxs-lookup"><span data-stu-id="e39cd-228">hello pipeline copies data from an Azure Storage account tooan Azure SQL database specified via parameters.</span></span> <span data-ttu-id="e39cd-229">toouse hello stejné šablony toodeploy objekt pro vytváření dat entity toodifferent prostředí, můžete vytvořit soubor parametrů pro každé prostředí a použít je při nasazování toothat prostředí.</span><span class="sxs-lookup"><span data-stu-id="e39cd-229">toouse hello same template toodeploy Data Factory entities toodifferent environments, you create a parameter file for each environment and use it when deploying toothat environment.</span></span>     

<span data-ttu-id="e39cd-230">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e39cd-230">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

<span data-ttu-id="e39cd-231">Všimněte si, že hello první příkaz používá parametr souboru hello vývojového prostředí, druhá pro hello testovací prostředí a hello třetí jeden hello produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="e39cd-231">Notice that hello first command uses parameter file for hello development environment, second one for hello test environment, and hello third one for hello production environment.</span></span>  

<span data-ttu-id="e39cd-232">Také můžete opakovaně použít hello šablony tooperform opakující úlohy.</span><span class="sxs-lookup"><span data-stu-id="e39cd-232">You can also reuse hello template tooperform repeated tasks.</span></span> <span data-ttu-id="e39cd-233">Například musíte toocreate mnoho objektů pro vytváření dat s jednou nebo více kanálů, které implementují hello stejné logiky, ale každý objekt pro vytváření dat používá různé účty úložiště a databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="e39cd-233">For example, you need toocreate many data factories with one or more pipelines that implement hello same logic but each data factory uses different Storage and SQL Database accounts.</span></span> <span data-ttu-id="e39cd-234">V tomto scénáři použijete hello stejné šablony v hello stejné prostředí (vývojářů, testovací nebo produkční) s jiným parametrem soubory toocreate datové továrny.</span><span class="sxs-lookup"><span data-stu-id="e39cd-234">In this scenario, you use hello same template in hello same environment (dev, test, or production) with different parameter files toocreate data factories.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="e39cd-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e39cd-235">Next steps</span></span>
<span data-ttu-id="e39cd-236">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="e39cd-236">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="e39cd-237">Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello:</span><span class="sxs-lookup"><span data-stu-id="e39cd-237">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="e39cd-238">toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="e39cd-238">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
