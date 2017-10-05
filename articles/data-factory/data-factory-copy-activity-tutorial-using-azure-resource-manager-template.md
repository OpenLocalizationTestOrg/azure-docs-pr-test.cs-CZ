---
title: "Kurz: Vytvoření kanálu pomocí šablony Resource Manageru | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory pomocí šablony Azure Resource Manageru. Tento kanál kopíruje data ze služby Azure Blob Storage do služby Azure SQL Database."
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
ms.openlocfilehash: 8a155213ed17e516a5c46abbe3d8a2bcc52268ed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-use-azure-resource-manager-template-to-create-a-data-factory-pipeline-to-copy-data"></a><span data-ttu-id="6d997-104">Kurz: Použití šablony Azure Resource Manageru k vytvoření kanálu Data Factory pro kopírování dat</span><span class="sxs-lookup"><span data-stu-id="6d997-104">Tutorial: Use Azure Resource Manager template to create a Data Factory pipeline to copy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6d997-105">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="6d997-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="6d997-106">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="6d997-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="6d997-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6d997-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="6d997-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d997-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="6d997-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6d997-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="6d997-110">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6d997-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="6d997-111">REST API</span><span class="sxs-lookup"><span data-stu-id="6d997-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="6d997-112">.NET API</span><span class="sxs-lookup"><span data-stu-id="6d997-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="6d997-113">V tomto kurzu je uvedeno, jak použít šablonu Azure Resource Manageru k vytvoření datové továrny Azure.</span><span class="sxs-lookup"><span data-stu-id="6d997-113">This tutorial shows you how to use an Azure Resource Manager template to create an Azure data factory.</span></span> <span data-ttu-id="6d997-114">Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="6d997-114">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="6d997-115">Neprovádí transformaci vstupních dat, aby vytvořil výstupní data.</span><span class="sxs-lookup"><span data-stu-id="6d997-115">It does not transform input data to produce output data.</span></span> <span data-ttu-id="6d997-116">Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="6d997-116">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="6d997-117">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="6d997-117">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="6d997-118">Aktivita kopírování kopíruje data z podporovaného úložiště dat do podporovaného úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="6d997-118">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="6d997-119">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="6d997-119">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="6d997-120">Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="6d997-120">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="6d997-121">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="6d997-121">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="6d997-122">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="6d997-122">A pipeline can have more than one activity.</span></span> <span data-ttu-id="6d997-123">A dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="6d997-123">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="6d997-124">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="6d997-124">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="6d997-125">Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="6d997-125">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="6d997-126">Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="6d997-126">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6d997-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d997-127">Prerequisites</span></span>
* <span data-ttu-id="6d997-128">Projděte si [Přehled a požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a proveďte **nutné** kroky.</span><span class="sxs-lookup"><span data-stu-id="6d997-128">Go through [Tutorial Overview and Prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete the **prerequisite** steps.</span></span>
* <span data-ttu-id="6d997-129">Podle pokynů v článku [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) si na počítač nainstalujte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6d997-129">Follow instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) article to install latest version of Azure PowerShell on your computer.</span></span> <span data-ttu-id="6d997-130">V tomto kurzu použijete prostředí PowerShell k nasazení entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6d997-130">In this tutorial, you use PowerShell to deploy Data Factory entities.</span></span> 
* <span data-ttu-id="6d997-131">(volitelné) Informace o šablonách Azure Resource Manageru najdete v tématu [Vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6d997-131">(optional) See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) to learn about Azure Resource Manager templates.</span></span>

## <a name="in-this-tutorial"></a><span data-ttu-id="6d997-132">V tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="6d997-132">In this tutorial</span></span>
<span data-ttu-id="6d997-133">V tomto kurzu vytvoříte datovou továrnu s následujícími entitami služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="6d997-133">In this tutorial, you create a data factory with the following Data Factory entities:</span></span>

| <span data-ttu-id="6d997-134">Entita</span><span class="sxs-lookup"><span data-stu-id="6d997-134">Entity</span></span> | <span data-ttu-id="6d997-135">Popis</span><span class="sxs-lookup"><span data-stu-id="6d997-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6d997-136">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6d997-136">Azure Storage linked service</span></span> |<span data-ttu-id="6d997-137">Propojí účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="6d997-137">Links your Azure Storage account to the data factory.</span></span> <span data-ttu-id="6d997-138">V tomto kurzu je služba Azure Storage zdrojové úložiště dat a služba Azure SQL Database je úložiště dat jímky pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="6d997-138">Azure Storage is the source data store and Azure SQL database is the sink data store for the copy activity in the tutorial.</span></span> <span data-ttu-id="6d997-139">Určuje účet úložiště, který obsahuje vstupní data pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="6d997-139">It specifies the storage account that contains the input data for the copy activity.</span></span> |
| <span data-ttu-id="6d997-140">Propojená služba Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="6d997-140">Azure SQL Database linked service</span></span> |<span data-ttu-id="6d997-141">Propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="6d997-141">Links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="6d997-142">Určuje službu Azure SQL Database, která uchovává výstupní data pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="6d997-142">It specifies the Azure SQL database that holds the output data for the copy activity.</span></span> |
| <span data-ttu-id="6d997-143">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="6d997-143">Azure Blob input dataset</span></span> |<span data-ttu-id="6d997-144">Odkazuje na propojenou službu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6d997-144">Refers to the Azure Storage linked service.</span></span> <span data-ttu-id="6d997-145">Propojená služba odkazuje na účet služby Azure Storage a datová sada Azure Blob určuje kontejner, složku a název souboru v úložišti, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="6d997-145">The linked service refers to an Azure Storage account and the Azure Blob dataset specifies the container, folder, and file name in the storage that holds the input data.</span></span> |
| <span data-ttu-id="6d997-146">Výstupní datová sada Azure SQL</span><span class="sxs-lookup"><span data-stu-id="6d997-146">Azure SQL output dataset</span></span> |<span data-ttu-id="6d997-147">Odkazuje na propojenou službu Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="6d997-147">Refers to the Azure SQL linked service.</span></span> <span data-ttu-id="6d997-148">Propojená služba Azure SQL odkazuje na server Azure SQL a datová sada Azure SQL určuje název tabulky, která obsahuje výstupní data.</span><span class="sxs-lookup"><span data-stu-id="6d997-148">The Azure SQL linked service refers to an Azure SQL server and the Azure SQL dataset specifies the name of the table that holds the output data.</span></span> |
| <span data-ttu-id="6d997-149">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="6d997-149">Data pipeline</span></span> |<span data-ttu-id="6d997-150">Kanál má jednu aktivitu typu Kopírování, která přijímá datovou sadu Azure Blob jako vstup a datovou sadu Azure SQL jako výstup.</span><span class="sxs-lookup"><span data-stu-id="6d997-150">The pipeline has one activity of type Copy that takes the Azure blob dataset as an input and the Azure SQL dataset as an output.</span></span> <span data-ttu-id="6d997-151">Aktivita kopírování kopíruje data z Azure Blob do tabulky ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6d997-151">The copy activity copies data from an Azure blob to a table in the Azure SQL database.</span></span> |

<span data-ttu-id="6d997-152">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="6d997-152">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="6d997-153">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="6d997-153">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="6d997-154">Existují dva typy aktivit: [aktivity přesunu dat](data-factory-data-movement-activities.md) a [transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="6d997-154">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="6d997-155">V tomto kurzu vytvoříte kanál s jednou aktivitou (aktivita kopírování).</span><span class="sxs-lookup"><span data-stu-id="6d997-155">In this tutorial, you create a pipeline with one activity (copy activity).</span></span>

![Kopírování Azure Blob do služby Azure SQL Database](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

<span data-ttu-id="6d997-157">Následující oddíl poskytuje hotovou šablonu Resource Manageru pro definování entit služby Data Factory, abyste mohli rychle projít kurzem a otestovat šablonu.</span><span class="sxs-lookup"><span data-stu-id="6d997-157">The following section provides the complete Resource Manager template for defining Data Factory entities so that you can quickly run through the tutorial and test the template.</span></span> <span data-ttu-id="6d997-158">Pro lepší pochopení toho, jak jsou jednotlivé entity služby Data Factory definovány, přejděte k oddílu [Entity služby Data Factory v šabloně](#data-factory-entities-in-the-template).</span><span class="sxs-lookup"><span data-stu-id="6d997-158">To understand how each Data Factory entity is defined, see [Data Factory entities in the template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="6d997-159">Šablona JSON služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="6d997-159">Data Factory JSON template</span></span>
<span data-ttu-id="6d997-160">Šablona Resource Manageru nejvyšší úrovně pro definování datové továrny je:</span><span class="sxs-lookup"><span data-stu-id="6d997-160">The top-level Resource Manager template for defining a data factory is:</span></span> 

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
<span data-ttu-id="6d997-161">Ve složce **C:\ADFGetStarted** vytvořte soubor JSON s názvem **ADFCopyTutorialARM.json** s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="6d997-161">Create a JSON file named **ADFCopyTutorialARM.json** in **C:\ADFGetStarted** folder with the following content:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
      "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
      "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
      "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
      "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
      "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
      "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
      "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
      "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
      "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
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
              "start": "2017-05-11T00:00:00Z",
              "end": "2017-05-12T00:00:00Z"
            }
          }
        ]
      }
    ]
  }
```

## <a name="parameters-json"></a><span data-ttu-id="6d997-162">Parametry JSON</span><span class="sxs-lookup"><span data-stu-id="6d997-162">Parameters JSON</span></span>
<span data-ttu-id="6d997-163">Vytvořte soubor JSON s názvem **ADFCopyTutorialARM-Parameters.json**, který obsahuje parametry pro šablonu Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="6d997-163">Create a JSON file named **ADFCopyTutorialARM-Parameters.json** that contains parameters for the Azure Resource Manager template.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6d997-164">Zadejte název a klíč svého účtu služby Azure Storage v parametrech storageAccountName a storageAccountKey.</span><span class="sxs-lookup"><span data-stu-id="6d997-164">Specify name and key of your Azure Storage account for storageAccountName and storageAccountKey parameters.</span></span>  
> 
> <span data-ttu-id="6d997-165">Zadejte server SQL Azure, databázi, uživatele a heslo v parametrech sqlServerName, databaseName, sqlServerUserName, a sqlServerPassword.</span><span class="sxs-lookup"><span data-stu-id="6d997-165">Specify Azure SQL server, database, user, and password for sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters.</span></span>  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "storageAccountName": { "value": "<Name of the Azure storage account>"    },
        "storageAccountKey": {
            "value": "<Key for the Azure storage account>"
        },
        "sourceBlobContainer": { "value": "adftutorial" },
        "sourceBlobName": { "value": "emp.txt" },
        "sqlServerName": { "value": "<Name of the Azure SQL server>" },
        "databaseName": { "value": "<Name of the Azure SQL database>" },
        "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
        "sqlServerPassword": { "value": "<password for the user>" },
        "targetSQLTable": { "value": "emp" }
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="6d997-166">Můžete mít samostatné soubory JSON s parametry pro vývojové, testovací a produkční prostředí, které lze použít se stejnou šablonou JSON služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6d997-166">You may have separate parameter JSON files for development, testing, and production environments that you can use with the same Data Factory JSON template.</span></span> <span data-ttu-id="6d997-167">Pomocí skriptu prostředí PowerShell můžete zautomatizovat nasazování entit služby Data Factory v těchto prostředích.</span><span class="sxs-lookup"><span data-stu-id="6d997-167">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span>  
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="6d997-168">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="6d997-168">Create data factory</span></span>
1. <span data-ttu-id="6d997-169">Otevřete prostředí **Azure PowerShell** a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6d997-169">Start **Azure PowerShell** and run the following command:</span></span>
   * <span data-ttu-id="6d997-170">Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="6d997-170">Run the following command and enter the user name and password that you use to sign in to the Azure portal.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount    
    ```  
   * <span data-ttu-id="6d997-171">Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="6d997-171">Run the following command to view all the subscriptions for this account.</span></span>
   
    ```PowerShell
    Get-AzureRmSubscription
    ```   
   * <span data-ttu-id="6d997-172">Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat.</span><span class="sxs-lookup"><span data-stu-id="6d997-172">Run the following command to select the subscription that you want to work with.</span></span>
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. <span data-ttu-id="6d997-173">Spuštěním následujícího příkazu nasaďte entity služby Data Factory pomocí šablony Resource Manageru, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="6d997-173">Run the following command to deploy Data Factory entities using the Resource Manager template you created in Step 1.</span></span>

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="6d997-174">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="6d997-174">Monitor pipeline</span></span>

1. <span data-ttu-id="6d997-175">Přihlaste se na webu [Azure Portal](https://portal.azure.com) pomocí svého účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d997-175">Log in to the [Azure portal](https://portal.azure.com) using your Azure account.</span></span>
2. <span data-ttu-id="6d997-176">V nabídce vlevo klikněte na **Datové továrny** (nebo) klikněte na **Další služby** a v kategorii **Inteligence a analýza** klikněte na **Datové továrny**.</span><span class="sxs-lookup"><span data-stu-id="6d997-176">Click **Data factories** on the left menu (or) click **More services** and click **Data factories** under **INTELLIGENCE + ANALYTICS** category.</span></span>
   
    ![Nabídka Datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. <span data-ttu-id="6d997-178">Na stránce **Datové továrny** vyhledejte svou datovou továrnu (AzureBlobToAzureSQLDatabaseDF).</span><span class="sxs-lookup"><span data-stu-id="6d997-178">In the **Data factories** page, search for and find your data factory (AzureBlobToAzureSQLDatabaseDF).</span></span> 
   
    ![Vyhledávání datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. <span data-ttu-id="6d997-180">Klikněte na svou datovou továrnu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d997-180">Click your Azure data factory.</span></span> <span data-ttu-id="6d997-181">Zobrazí se domovská stránka datové továrny.</span><span class="sxs-lookup"><span data-stu-id="6d997-181">You see the home page for the data factory.</span></span>
   
    ![Domovská stránka datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. <span data-ttu-id="6d997-183">Postupujte podle pokynů v tématu [Monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) k monitorování kanálu a datových sad, které jste vytvořili v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6d997-183">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) to monitor the pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="6d997-184">V současné době Visual Studio monitorování kanálů Data Factory nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="6d997-184">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span>
7. <span data-ttu-id="6d997-185">Když je řez ve stavu **Připraveno**, ověřte zkopírování dat do tabulky **emp** ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6d997-185">When a slice is in the **Ready** state, verify that the data is copied to the **emp** table in the Azure SQL database.</span></span>


<span data-ttu-id="6d997-186">Další informace o používání oken portálu Azure Portal k monitorování kanálu a datových sad, které jste vytvořili v tomto kurzu, najdete v článku [Monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="6d997-186">For more information on how to use Azure portal blades to monitor pipeline and datasets you have created in this tutorial, see [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) .</span></span>

<span data-ttu-id="6d997-187">Další informace o používání aplikace pro monitorování a správu k monitorování datových kanálů najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí monitorovací aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="6d997-187">For more information on how to use the Monitor & Manage application to monitor your data pipelines, see [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md).</span></span>

## <a name="data-factory-entities-in-the-template"></a><span data-ttu-id="6d997-188">Entity služby Data Factory v šabloně</span><span class="sxs-lookup"><span data-stu-id="6d997-188">Data Factory entities in the template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="6d997-189">Definování datové továrny</span><span class="sxs-lookup"><span data-stu-id="6d997-189">Define data factory</span></span>
<span data-ttu-id="6d997-190">Datovou továrnu definujete v šabloně Resource Manageru, jak je znázorněno v následující ukázce:</span><span class="sxs-lookup"><span data-stu-id="6d997-190">You define a data factory in the Resource Manager template as shown in the following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```

<span data-ttu-id="6d997-191">Hodnota dataFactoryName je definována takto:</span><span class="sxs-lookup"><span data-stu-id="6d997-191">The dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

<span data-ttu-id="6d997-192">Je to jedinečný řetězec vycházející z ID skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6d997-192">It is a unique string based on the resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="6d997-193">Definování entit služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="6d997-193">Defining Data Factory entities</span></span>
<span data-ttu-id="6d997-194">V šabloně JSON jsou definovány následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="6d997-194">The following Data Factory entities are defined in the JSON template:</span></span> 

1. [<span data-ttu-id="6d997-195">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6d997-195">Azure Storage linked service</span></span>](#azure-storage-linked-service)
2. [<span data-ttu-id="6d997-196">Propojená služba Azure SQL</span><span class="sxs-lookup"><span data-stu-id="6d997-196">Azure SQL linked service</span></span>](#azure-sql-database-linked-service)
3. [<span data-ttu-id="6d997-197">Datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="6d997-197">Azure blob dataset</span></span>](#azure-blob-dataset)
4. [<span data-ttu-id="6d997-198">Datová sada Azure SQL</span><span class="sxs-lookup"><span data-stu-id="6d997-198">Azure SQL dataset</span></span>](#azure-sql-dataset)
5. [<span data-ttu-id="6d997-199">Data Pipeline s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="6d997-199">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="6d997-200">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6d997-200">Azure Storage linked service</span></span>
<span data-ttu-id="6d997-201">Služba AzureStorageLinkedService propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="6d997-201">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="6d997-202">V rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) jste vytvořili kontejner a nahráli data do tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6d997-202">You created a container and uploaded data to this storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="6d997-203">V tomto oddílu zadáte název a klíč svého účtu služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6d997-203">You specify the name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="6d997-204">Podrobnosti o vlastnostech JSON sloužících k definování propojené služby Azure Storage najdete v oddílu [Propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span><span class="sxs-lookup"><span data-stu-id="6d997-204">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used to define an Azure Storage linked service.</span></span> 

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

<span data-ttu-id="6d997-205">Vlastnost connectionString používá parametry storageAccountName a storageAccountKey.</span><span class="sxs-lookup"><span data-stu-id="6d997-205">The connectionString uses the storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="6d997-206">Hodnoty těchto parametrů se předávají pomocí konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="6d997-206">The values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="6d997-207">Definice také používá proměnné azureStorageLinkedService a dataFactoryName definované v šabloně.</span><span class="sxs-lookup"><span data-stu-id="6d997-207">The definition also uses variables: azureStroageLinkedService and dataFactoryName defined in the template.</span></span> 

#### <a name="azure-sql-database-linked-service"></a><span data-ttu-id="6d997-208">Propojená služba Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="6d997-208">Azure SQL Database linked service</span></span>
<span data-ttu-id="6d997-209">Služba AzureSqlLinkedService propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="6d997-209">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="6d997-210">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="6d997-210">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="6d997-211">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku emp.</span><span class="sxs-lookup"><span data-stu-id="6d997-211">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="6d997-212">V tomto oddílu zadáte název serveru Azure SQL, název databáze, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="6d997-212">You specify the Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="6d997-213">Podrobnosti o vlastnostech JSON sloužících k definování propojené služby Azure SQL najdete v oddílu [Propojená služba Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6d997-213">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used to define an Azure SQL linked service.</span></span>  

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

<span data-ttu-id="6d997-214">Vlastnost connectionString používá parametry sqlServerName, databaseName, sqlServerUserName a sqlServerPassword, jejichž hodnoty se předávají pomocí konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="6d997-214">The connectionString uses sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters whose values are passed by using a configuration file.</span></span> <span data-ttu-id="6d997-215">Definice také používá tyto proměnné z šablony: azureSqlLinkedServiceName a dataFactoryName.</span><span class="sxs-lookup"><span data-stu-id="6d997-215">The definition also uses the following variables from the template: azureSqlLinkedServiceName, dataFactoryName.</span></span>

#### <a name="azure-blob-dataset"></a><span data-ttu-id="6d997-216">Datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="6d997-216">Azure blob dataset</span></span>
<span data-ttu-id="6d997-217">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6d997-217">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="6d997-218">V definici datové sady Azure Blob zadáte názvy kontejneru objektů blob, složky a souboru, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="6d997-218">In Azure blob dataset definition, you specify names of blob container, folder, and file that contains the input data.</span></span> <span data-ttu-id="6d997-219">Podrobnosti o vlastnostech JSON sloužících k definování datové sady Azure Blob najdete v oddílu [Vlastnosti datové sady Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6d997-219">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span> 

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

#### <a name="azure-sql-dataset"></a><span data-ttu-id="6d997-220">Datová sada Azure SQL</span><span class="sxs-lookup"><span data-stu-id="6d997-220">Azure SQL dataset</span></span>
<span data-ttu-id="6d997-221">Zadáte název tabulky ve službě Azure SQL Database, která uchovává zkopírovaná data ze služby Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6d997-221">You specify the name of the table in the Azure SQL database that holds the copied data from the Azure Blob storage.</span></span> <span data-ttu-id="6d997-222">Podrobnosti o vlastnostech JSON sloužících k definování datové sady Azure SQL najdete v oddílu [Vlastnosti datové sady Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6d997-222">See [Azure SQL dataset properties](data-factory-azure-sql-connector.md#dataset-properties) for details about JSON properties used to define an Azure SQL dataset.</span></span> 

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

#### <a name="data-pipeline"></a><span data-ttu-id="6d997-223">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="6d997-223">Data pipeline</span></span>
<span data-ttu-id="6d997-224">Nadefinujete kanál, který kopíruje data z datové sady Azure Blob do datové sady Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="6d997-224">You define a pipeline that copies data from the Azure blob dataset to the Azure SQL dataset.</span></span> <span data-ttu-id="6d997-225">Popisy elementů JSON sloužících k definování kanálu v tomto příkladu najdete v oddílu [Kód JSON kanálu](data-factory-create-pipelines.md#pipeline-json).</span><span class="sxs-lookup"><span data-stu-id="6d997-225">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used to define a pipeline in this example.</span></span> 

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
          "start": "2017-05-11T00:00:00Z",
          "end": "2017-05-12T00:00:00Z"
    }
}
```

## <a name="reuse-the-template"></a><span data-ttu-id="6d997-226">Znovupoužití šablony</span><span class="sxs-lookup"><span data-stu-id="6d997-226">Reuse the template</span></span>
<span data-ttu-id="6d997-227">V tomto kurzu jste vytvořili šablonu pro definování entit služby Data Factory a šablonu pro předávání hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="6d997-227">In the tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="6d997-228">Kanál kopíruje data z účtu služby Azure Storage do služby Azure SQL Database a tyto služby jsou určeny prostřednictvím parametrů.</span><span class="sxs-lookup"><span data-stu-id="6d997-228">The pipeline copies data from an Azure Storage account to an Azure SQL database specified via parameters.</span></span> <span data-ttu-id="6d997-229">Chcete-li použít stejnou šablonu k nasazení entit služby Data Factory do různých prostředí, vytvořte pro každé prostředí soubor parametrů a použijte jej při nasazování příslušného prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d997-229">To use the same template to deploy Data Factory entities to different environments, you create a parameter file for each environment and use it when deploying to that environment.</span></span>     

<span data-ttu-id="6d997-230">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6d997-230">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

<span data-ttu-id="6d997-231">Všimněte si, že první příkaz používá soubor parametrů pro vývojové prostředí, druhý příkaz používá soubor parametrů pro testovací prostředí a třetí příkaz používá soubor parametrů pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d997-231">Notice that the first command uses parameter file for the development environment, second one for the test environment, and the third one for the production environment.</span></span>  

<span data-ttu-id="6d997-232">Šablonu můžete také znovu použít k provádění opakujících se úloh.</span><span class="sxs-lookup"><span data-stu-id="6d997-232">You can also reuse the template to perform repeated tasks.</span></span> <span data-ttu-id="6d997-233">Například: Potřebujete vytvořit mnoho datových továren s jedním nebo více kanály, které implementují stejnou logiku, ale každá datová továrna používá jiný účet služby Storage a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6d997-233">For example, you need to create many data factories with one or more pipelines that implement the same logic but each data factory uses different Storage and SQL Database accounts.</span></span> <span data-ttu-id="6d997-234">V tomto scénáři použijete k vytvoření datových továren stejnou šablonu ve stejném prostředí (vývojové, testovací nebo produkční) s různými soubory parametrů.</span><span class="sxs-lookup"><span data-stu-id="6d997-234">In this scenario, you use the same template in the same environment (dev, test, or production) with different parameter files to create data factories.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="6d997-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d997-235">Next steps</span></span>
<span data-ttu-id="6d997-236">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="6d997-236">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="6d997-237">Následující tabulka obsahuje seznam úložišť dat podporovaných jako zdroje a cíle aktivitou kopírování:</span><span class="sxs-lookup"><span data-stu-id="6d997-237">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="6d997-238">Kliknutím na odkaz úložiště dat v tabulce získáte další informace o kopírování dat do nebo z úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="6d997-238">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span>
