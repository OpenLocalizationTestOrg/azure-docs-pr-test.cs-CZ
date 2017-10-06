---
title: "aaaBuild vaše první objekt pro vytváření dat (šablony Resource Manageru) | Microsoft Docs"
description: "V tomto kurzu vytvoříte ukázkový kanál služby Azure Data Factory pomocí šablony Azure Resource Manageru."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: eb9e70b9-a13a-4a27-8256-2759496be470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fa08cd1ac3a0e5c5bf4bd4c6bd9dfa6dba9f4319
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a><span data-ttu-id="3349e-103">Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="3349e-103">Tutorial: Build your first Azure data factory using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3349e-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="3349e-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="3349e-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3349e-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="3349e-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3349e-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="3349e-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3349e-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="3349e-108">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="3349e-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="3349e-109">REST API</span><span class="sxs-lookup"><span data-stu-id="3349e-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
> 
> 

<span data-ttu-id="3349e-110">V tomto článku je použít toocreate šablony Azure Resource Manager první objekt pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="3349e-110">In this article, you use an Azure Resource Manager template toocreate your first Azure data factory.</span></span> <span data-ttu-id="3349e-111">kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3349e-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="3349e-112">Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="3349e-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="3349e-113">Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="3349e-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="3349e-114">Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="3349e-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="3349e-115">Hello datovém kanálu v tomto kurzu transformuje vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="3349e-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="3349e-116">Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3349e-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="3349e-117">Hello kanálu v tomto kurzu má jenom jedna aktivita typu: HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="3349e-117">hello pipeline in this tutorial has only one activity of type: HDInsightHive.</span></span> <span data-ttu-id="3349e-118">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="3349e-118">A pipeline can have more than one activity.</span></span> <span data-ttu-id="3349e-119">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="3349e-119">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="3349e-120">Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="3349e-120">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3349e-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3349e-121">Prerequisites</span></span>
* <span data-ttu-id="3349e-122">Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="3349e-122">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="3349e-123">Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku tooinstall nejnovější verzi prostředí Azure PowerShell ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3349e-123">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span>
* <span data-ttu-id="3349e-124">V tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) toolearn o šablonách Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3349e-124">See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn about Azure Resource Manager templates.</span></span> 

## <a name="in-this-tutorial"></a><span data-ttu-id="3349e-125">V tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="3349e-125">In this tutorial</span></span>
| <span data-ttu-id="3349e-126">Entita</span><span class="sxs-lookup"><span data-stu-id="3349e-126">Entity</span></span> | <span data-ttu-id="3349e-127">Popis</span><span class="sxs-lookup"><span data-stu-id="3349e-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3349e-128">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3349e-128">Azure Storage linked service</span></span> |<span data-ttu-id="3349e-129">Propojí datovou továrnu toohello účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3349e-129">Links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="3349e-130">blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="3349e-130">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> |
| <span data-ttu-id="3349e-131">Propojená služba HDInsightu na vyžádání</span><span class="sxs-lookup"><span data-stu-id="3349e-131">HDInsight on-demand linked service</span></span> |<span data-ttu-id="3349e-132">Propojení služby na vyžádání HDInsight clusteru toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="3349e-132">Links an on-demand HDInsight cluster toohello data factory.</span></span> <span data-ttu-id="3349e-133">Hello clusteru se automaticky vytvoří za vás tooprocess dat a odstraní se po dokončení zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-133">hello cluster is automatically created for you tooprocess data and is deleted after hello processing is done.</span></span> |
| <span data-ttu-id="3349e-134">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3349e-134">Azure Blob input dataset</span></span> |<span data-ttu-id="3349e-135">Odkazuje toohello propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3349e-135">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="3349e-136">Hello propojená služba odkazuje tooan účtu Azure Storage a datové sady objektu Blob Azure hello určuje hello kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-136">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello input data.</span></span> |
| <span data-ttu-id="3349e-137">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3349e-137">Azure Blob output dataset</span></span> |<span data-ttu-id="3349e-138">Odkazuje toohello propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3349e-138">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="3349e-139">Hello propojená služba odkazuje tooan účtu Azure Storage a datové sady objektu Blob Azure hello určuje hello kontejner, složku a název souboru v úložišti hello, který obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="3349e-139">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello output data.</span></span> |
| <span data-ttu-id="3349e-140">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="3349e-140">Data pipeline</span></span> |<span data-ttu-id="3349e-141">Hello kanálu má jednu aktivitu typu HDInsightHive, který využívá hello vstupní datové sady a vytváří hello výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3349e-141">hello pipeline has one activity of type HDInsightHive, which consumes hello input dataset and produces hello output dataset.</span></span> |

<span data-ttu-id="3349e-142">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="3349e-142">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="3349e-143">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="3349e-143">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="3349e-144">Existují dva typy aktivit: [aktivity přesunu dat](data-factory-data-movement-activities.md) a [transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="3349e-144">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="3349e-145">V tomto kurzu vytvoříte kanál s jednou aktivitou (aktivita Hive).</span><span class="sxs-lookup"><span data-stu-id="3349e-145">In this tutorial, you create a pipeline with one activity (Hive activity).</span></span>

<span data-ttu-id="3349e-146">Hello následující část obsahuje hello dokončení šablony Resource Manageru pro definování entit služby Data Factory, takže můžete rychle spustit prostřednictvím hello kurzu a testování hello šablony.</span><span class="sxs-lookup"><span data-stu-id="3349e-146">hello following section provides hello complete Resource Manager template for defining Data Factory entities so that you can quickly run through hello tutorial and test hello template.</span></span> <span data-ttu-id="3349e-147">toounderstand o každé entity služby Data Factory je definován, v tématu [entit služby Data Factory v šabloně hello](#data-factory-entities-in-the-template) části.</span><span class="sxs-lookup"><span data-stu-id="3349e-147">toounderstand how each Data Factory entity is defined, see [Data Factory entities in hello template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="3349e-148">Šablona JSON služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="3349e-148">Data Factory JSON template</span></span>
<span data-ttu-id="3349e-149">Hello nejvyšší úrovně šablony Resource Manageru pro definování objekt pro vytváření dat je:</span><span class="sxs-lookup"><span data-stu-id="3349e-149">hello top-level Resource Manager template for defining a data factory is:</span></span> 

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
<span data-ttu-id="3349e-150">Vytvořte soubor JSON s názvem **C:\adfgetstarted** v **C:\ADFGetStarted** složku s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="3349e-150">Create a JSON file named **ADFTutorialARM.json** in **C:\ADFGetStarted** folder with hello following content:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "storageAccountName": { "type": "string", "metadata": { "description": "Name of hello Azure storage account that contains hello input/output data." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for hello Azure storage account." } },
          "blobContainer": { "type": "string", "metadata": { "description": "Name of hello blob container in hello Azure Storage account." } },
          "inputBlobFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that has hello input file." } },
          "inputBlobName": { "type": "string", "metadata": { "description": "Name of hello input file/blob." } },
          "outputBlobFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that will hold hello transformed data." } },
          "hiveScriptFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that contains hello Hive query file." } },
          "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of hello hive query (HQL) file." } }
    },
    "variables": {
          "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
          "blobInputDatasetName": "AzureBlobInput",
          "blobOutputDatasetName": "AzureBlobOutput",
          "pipelineName": "HiveTransformPipeline"
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
            "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "HDInsightOnDemand",
                  "typeProperties": {
                    "version": "3.5",
                    "clusterSize": 1,
                    "timeToLive": "00:05:00",
                    "osType": "Linux",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
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
                  "typeProperties": {
                    "fileName": "[parameters('inputBlobName')]",
                    "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                    "format": {
                          "type": "TextFormat",
                          "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Month",
                    "interval": 1
                  },
                  "external": true
            }
          },
          {
            "type": "datasets",
            "name": "[variables('blobOutputDatasetName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "typeProperties": {
                    "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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
          },
          {
            "type": "datapipelines",
            "name": "[variables('pipelineName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]",
                  "[variables('hdInsightOnDemandLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('blobOutputDatasetName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "description": "Pipeline that transforms data using Hive script.",
                  "activities": [
                {
                      "type": "HDInsightHive",
                      "typeProperties": {
                        "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                        "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                        "defines": {
                              "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                              "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                        }
                      },
                      "inputs": [
                        {
                              "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                              "name": "[variables('blobOutputDatasetName')]"
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
                      "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                }
                  ],
                  "start": "2017-07-01T00:00:00Z",
                  "end": "2017-07-02T00:00:00Z",
                  "isPaused": false
              }
          }
        ]
      }
    ]
}
```

> [!NOTE]
> <span data-ttu-id="3349e-151">Další příklad šablony Resource Manageru pro vytváření datové továrny Azure najdete v části [Kurz: Vytvoření kanálu s aktivitou kopírování pomocí šablony Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="3349e-151">You can find another example of Resource Manager template for creating an Azure data factory on [Tutorial: Create a pipeline with Copy Activity using an Azure Resource Manager template](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span></span>  
> 
> 

## <a name="parameters-json"></a><span data-ttu-id="3349e-152">Parametry JSON</span><span class="sxs-lookup"><span data-stu-id="3349e-152">Parameters JSON</span></span>
<span data-ttu-id="3349e-153">Vytvořte soubor JSON s názvem **ADFTutorialARM Parameters.JSON tímto kódem** obsahující parametry šablony Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-153">Create a JSON file named **ADFTutorialARM-Parameters.json** that contains parameters for hello Azure Resource Manager template.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="3349e-154">Zadejte název hello a klíč účtu úložiště Azure pro hello **storageAccountName** a **storageAccountKey** parametry v tomto souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="3349e-154">Specify hello name and key of your Azure Storage account for hello **storageAccountName** and **storageAccountKey** parameters in this parameter file.</span></span> 
> 
> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "value": "<Name of your Azure Storage account>"
        },
        "storageAccountKey": {
            "value": "<Key of your Azure Storage account>"
        },
        "blobContainer": {
            "value": "adfgetstarted"
        },
        "inputBlobFolder": {
            "value": "inputdata"
        },
        "inputBlobName": {
            "value": "input.log"
        },
        "outputBlobFolder": {
            "value": "partitioneddata"
        },
        "hiveScriptFolder": {
              "value": "script"
        },
        "hiveScriptFile": {
              "value": "partitionweblogs.hql"
        }
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="3349e-155">Může mít soubory JSON samostatného parametru pro vývoj, testování a provozním prostředí, která můžete použít s hello stejné šablony Data Factory JSON.</span><span class="sxs-lookup"><span data-stu-id="3349e-155">You may have separate parameter JSON files for development, testing, and production environments that you can use with hello same Data Factory JSON template.</span></span> <span data-ttu-id="3349e-156">Pomocí skriptu prostředí PowerShell můžete zautomatizovat nasazování entit služby Data Factory v těchto prostředích.</span><span class="sxs-lookup"><span data-stu-id="3349e-156">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span> 
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="3349e-157">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="3349e-157">Create data factory</span></span>
1. <span data-ttu-id="3349e-158">Spustit **prostředí Azure PowerShell** a hello spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3349e-158">Start **Azure PowerShell** and run hello following command:</span></span> 
   * <span data-ttu-id="3349e-159">Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3349e-159">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
    ```PowerShell
    Login-AzureRmAccount
    ```  
   * <span data-ttu-id="3349e-160">Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="3349e-160">Run hello following command tooview all hello subscriptions for this account.</span></span>
    ```PowerShell
    Get-AzureRmSubscription
    ``` 
   * <span data-ttu-id="3349e-161">Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-161">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="3349e-162">Toto předplatné by měl být hello stejné jako hello jeden, které jste použili v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3349e-162">This subscription should be hello same as hello one you used in hello Azure portal.</span></span>
    ```
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```   
2. <span data-ttu-id="3349e-163">Spusťte hello následující entity služby Data Factory toodeploy příkaz pomocí šablony Resource Manageru hello, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="3349e-163">Run hello following command toodeploy Data Factory entities using hello Resource Manager template you created in Step 1.</span></span> 

    ```PowerShell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="3349e-164">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="3349e-164">Monitor pipeline</span></span>
1. <span data-ttu-id="3349e-165">Po přihlášení toohello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet** a vyberte **datové továrny**.</span><span class="sxs-lookup"><span data-stu-id="3349e-165">After logging in toohello [Azure portal](https://portal.azure.com/), Click **Browse** and select **Data factories**.</span></span>
     <span data-ttu-id="3349e-166">![Procházet -&gt; Datové továrny](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span><span class="sxs-lookup"><span data-stu-id="3349e-166">![Browse->Data factories](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span></span>
2. <span data-ttu-id="3349e-167">V hello **datové továrny** okně klikněte na objekt pro vytváření dat hello (**TutorialFactoryARM**) jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3349e-167">In hello **Data Factories** blade, click hello data factory (**TutorialFactoryARM**) you created.</span></span>    
3. <span data-ttu-id="3349e-168">V hello **Data Factory** objektu pro vytváření dat, klikněte na **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="3349e-168">In hello **Data Factory** blade for your data factory, click **Diagram**.</span></span>

     ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4. <span data-ttu-id="3349e-170">V hello **zobrazení diagramu**, zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3349e-170">In hello **Diagram View**, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>
   
   ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
5. <span data-ttu-id="3349e-172">V hello zobrazení diagramu, klikněte dvakrát na datovou sadu hello **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="3349e-172">In hello Diagram View, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="3349e-173">Uvidíte, že hello řez, který se právě zpracovává.</span><span class="sxs-lookup"><span data-stu-id="3349e-173">You see that hello slice that is currently being processed.</span></span>
   
    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
6. <span data-ttu-id="3349e-175">Po dokončení zpracování uvidíte hello řez ve **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="3349e-175">When processing is done, you see hello slice in **Ready** state.</span></span> <span data-ttu-id="3349e-176">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut).</span><span class="sxs-lookup"><span data-stu-id="3349e-176">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="3349e-177">Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.</span><span class="sxs-lookup"><span data-stu-id="3349e-177">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
   
    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png)    
7. <span data-ttu-id="3349e-179">Pokud je řez hello v **připraven** stavu, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="3349e-179">When hello slice is in **Ready** state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  

<span data-ttu-id="3349e-180">V tématu [monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md) pokyny, jak toouse hello oken Azure portal toomonitor hello kanálu a datových sad jste vytvořili v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3349e-180">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how toouse hello Azure portal blades toomonitor hello pipeline and datasets you have created in this tutorial.</span></span>

<span data-ttu-id="3349e-181">Můžete také použít monitorování a správě aplikací toomonitor datových kanálů.</span><span class="sxs-lookup"><span data-stu-id="3349e-181">You can also use Monitor and Manage App toomonitor your data pipelines.</span></span> <span data-ttu-id="3349e-182">V tématu [monitorování a Správa kanálů služby Azure Data Factory pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) podrobnosti o použití aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-182">See [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md) for details about using hello application.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="3349e-183">vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="3349e-183">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="3349e-184">Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="3349e-184">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
> 
> 

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="3349e-185">Entity objektu pro vytváření dat v šabloně hello</span><span class="sxs-lookup"><span data-stu-id="3349e-185">Data Factory entities in hello template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="3349e-186">Definování datové továrny</span><span class="sxs-lookup"><span data-stu-id="3349e-186">Define data factory</span></span>
<span data-ttu-id="3349e-187">Objekt pro vytváření dat v šabloně Resource Manager hello definovat, jak je uvedeno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3349e-187">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```
<span data-ttu-id="3349e-188">Hello dataFactoryName je definován jako:</span><span class="sxs-lookup"><span data-stu-id="3349e-188">hello dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
```
<span data-ttu-id="3349e-189">Jedná se o jedinečný řetězec na základě ID hello prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="3349e-189">It is a unique string based on hello resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="3349e-190">Definování entit služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="3349e-190">Defining Data Factory entities</span></span>
<span data-ttu-id="3349e-191">Hello následující entity služby Data Factory jsou definovány v šabloně hello JSON:</span><span class="sxs-lookup"><span data-stu-id="3349e-191">hello following Data Factory entities are defined in hello JSON template:</span></span> 

* [<span data-ttu-id="3349e-192">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3349e-192">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="3349e-193">Propojená služba HDInsightu na vyžádání</span><span class="sxs-lookup"><span data-stu-id="3349e-193">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="3349e-194">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3349e-194">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="3349e-195">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3349e-195">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="3349e-196">Data Pipeline s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="3349e-196">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="3349e-197">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3349e-197">Azure Storage linked service</span></span>
<span data-ttu-id="3349e-198">Zadejte název hello a klíč účtu úložiště Azure v této části.</span><span class="sxs-lookup"><span data-stu-id="3349e-198">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="3349e-199">V tématu [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3349e-199">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span> 

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
<span data-ttu-id="3349e-200">Hello **connectionString** používá hello storageAccountName a storageAccountKey parametry.</span><span class="sxs-lookup"><span data-stu-id="3349e-200">hello **connectionString** uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="3349e-201">Hello hodnoty pro tyto parametry předané pomocí konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="3349e-201">hello values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="3349e-202">definice Hello také používá proměnné: azureStroageLinkedService a dataFactoryName definovaná v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-202">hello definition also uses variables: azureStroageLinkedService and dataFactoryName defined in hello template.</span></span> 

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="3349e-203">Propojená služba HDInsightu na vyžádání</span><span class="sxs-lookup"><span data-stu-id="3349e-203">HDInsight on-demand linked service</span></span>
<span data-ttu-id="3349e-204">V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) článek podrobnosti o toodefine vlastnosti používané JSON propojené služby HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3349e-204">See [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used toodefine an HDInsight on-demand linked service.</span></span>  

```json
{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
        }
    }
}
```
<span data-ttu-id="3349e-205">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="3349e-205">Note hello following points:</span></span> 

* <span data-ttu-id="3349e-206">Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello výše JSON.</span><span class="sxs-lookup"><span data-stu-id="3349e-206">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello above JSON.</span></span> <span data-ttu-id="3349e-207">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="3349e-207">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span> 
* <span data-ttu-id="3349e-208">Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="3349e-208">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="3349e-209">Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).</span><span class="sxs-lookup"><span data-stu-id="3349e-209">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
* <span data-ttu-id="3349e-210">Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="3349e-210">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="3349e-211">HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-211">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="3349e-212">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="3349e-212">This behavior is by design.</span></span> <span data-ttu-id="3349e-213">S na vyžádání propojené služby HDInsight, HDInsight cluster vytvoří pokaždé, když řez musí toobe zpracování, pokud neexistuje aktivní cluster (**timeToLive**) a po dokončení zpracování hello, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="3349e-213">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>
  
    <span data-ttu-id="3349e-214">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="3349e-214">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="3349e-215">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="3349e-215">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="3349e-216">vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času".</span><span class="sxs-lookup"><span data-stu-id="3349e-216">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="3349e-217">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="3349e-217">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

<span data-ttu-id="3349e-218">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="3349e-218">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="3349e-219">Vstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3349e-219">Azure blob input dataset</span></span>
<span data-ttu-id="3349e-220">Zadáte názvy hello kontejner objektů blob, složku a soubor, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="3349e-220">You specify hello names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="3349e-221">V tématu [vlastnosti datové sady objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="3349e-221">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span> 

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
        "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="3349e-222">Tato definice používá hello následující parametry definované v parametru šablony: blobContainer, inputBlobFolder a inputBlobName.</span><span class="sxs-lookup"><span data-stu-id="3349e-222">This definition uses hello following parameters defined in parameter template: blobContainer, inputBlobFolder, and inputBlobName.</span></span> 

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="3349e-223">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3349e-223">Azure Blob output dataset</span></span>
<span data-ttu-id="3349e-224">Zadáte názvy hello kontejner objektů blob a složky, která obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="3349e-224">You specify hello names of blob container and folder that holds hello output data.</span></span> <span data-ttu-id="3349e-225">V tématu [vlastnosti datové sady objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="3349e-225">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>  

```json
{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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

<span data-ttu-id="3349e-226">Tato definice používá následující parametry definované v šabloně parametr hello hello: blobContainer a outputBlobFolder.</span><span class="sxs-lookup"><span data-stu-id="3349e-226">This definition uses hello following parameters defined in hello parameter template: blobContainer and outputBlobFolder.</span></span> 

#### <a name="data-pipeline"></a><span data-ttu-id="3349e-227">Data Pipeline</span><span class="sxs-lookup"><span data-stu-id="3349e-227">Data pipeline</span></span>
<span data-ttu-id="3349e-228">Definujete kanál, který převádí data spuštěním skriptu Hive v clusteru Azure HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3349e-228">You define a pipeline that transform data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="3349e-229">V tématu [JSON kanálu](data-factory-create-pipelines.md#pipeline-json) popisy toodefine prvky používané JSON kanálu v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="3349e-229">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span> 

```json
{
    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('hdInsightOnDemandLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('blobOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "description": "Pipeline that transforms data using Hive script.",
        "activities": [
        {
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                    "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                    "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
            },
            "inputs": [
            {
                "name": "[variables('blobInputDatasetName')]"
            }
            ],
            "outputs": [
            {
                "name": "[variables('blobOutputDatasetName')]"
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
            "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
        }
        ],
        "start": "2017-07-01T00:00:00Z",
        "end": "2017-07-02T00:00:00Z",
        "isPaused": false
    }
}
```

## <a name="reuse-hello-template"></a><span data-ttu-id="3349e-230">Opakované použití hello šablony</span><span class="sxs-lookup"><span data-stu-id="3349e-230">Reuse hello template</span></span>
<span data-ttu-id="3349e-231">V kurzu hello jste vytvořili šablonu pro definování entit služby Data Factory a šablonu pro předávání hodnot pro parametry.</span><span class="sxs-lookup"><span data-stu-id="3349e-231">In hello tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="3349e-232">toouse hello stejné šablony toodeploy objekt pro vytváření dat entity toodifferent prostředí, můžete vytvořit soubor parametrů pro každé prostředí a použít je při nasazování toothat prostředí.</span><span class="sxs-lookup"><span data-stu-id="3349e-232">toouse hello same template toodeploy Data Factory entities toodifferent environments, you create a parameter file for each environment and use it when deploying toothat environment.</span></span>     

<span data-ttu-id="3349e-233">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3349e-233">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```
<span data-ttu-id="3349e-234">Všimněte si, že hello první příkaz používá parametr souboru hello vývojového prostředí, druhá pro hello testovací prostředí a hello třetí jeden hello produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="3349e-234">Notice that hello first command uses parameter file for hello development environment, second one for hello test environment, and hello third one for hello production environment.</span></span>  

<span data-ttu-id="3349e-235">Také můžete opakovaně použít hello šablony tooperform opakující úlohy.</span><span class="sxs-lookup"><span data-stu-id="3349e-235">You can also reuse hello template tooperform repeated tasks.</span></span> <span data-ttu-id="3349e-236">Například musíte toocreate mnoho objektů pro vytváření dat s jednou nebo více kanálů, které implementují hello stejné logiky, ale každý data factory používá jiný úložiště Azure a účty Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3349e-236">For example, you need toocreate many data factories with one or more pipelines that implement hello same logic but each data factory uses different Azure storage and Azure SQL Database accounts.</span></span> <span data-ttu-id="3349e-237">V tomto scénáři použijete hello stejné šablony v hello stejné prostředí (vývojářů, testovací nebo produkční) s jiným parametrem soubory toocreate datové továrny.</span><span class="sxs-lookup"><span data-stu-id="3349e-237">In this scenario, you use hello same template in hello same environment (dev, test, or production) with different parameter files toocreate data factories.</span></span> 

## <a name="resource-manager-template-for-creating-a-gateway"></a><span data-ttu-id="3349e-238">Šablona Resource Manageru pro vytvoření brány</span><span class="sxs-lookup"><span data-stu-id="3349e-238">Resource Manager template for creating a gateway</span></span>
<span data-ttu-id="3349e-239">Zde je ukázka šablony Resource Manageru pro vytvoření logické brány v hello zpět.</span><span class="sxs-lookup"><span data-stu-id="3349e-239">Here is a sample Resource Manager template for creating a logical gateway in hello back.</span></span> <span data-ttu-id="3349e-240">Nainstalujte bránu do svého místního počítače nebo virtuálního počítače Azure IaaS a zaregistrujte bránu hello službě Data Factory pomocí klíče.</span><span class="sxs-lookup"><span data-stu-id="3349e-240">Install a gateway on your on-premises computer or Azure IaaS VM and register hello gateway with Data Factory service using a key.</span></span> <span data-ttu-id="3349e-241">Podrobnosti najdete v článku [Přesun dat mezi místním prostředím a cloudem](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="3349e-241">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) for details.</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
    },
    "variables": {
        "dataFactoryName":  "GatewayUsingArmDF",
        "apiVersion": "2015-10-01",
        "singleQuote": "'"
    },
    "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "eastus",
            "resources": [
                {
                    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                    "type": "gateways",
                    "apiVersion": "[variables('apiVersion')]",
                    "name": "GatewayUsingARM",
                    "properties": {
                        "description": "my gateway"
                    }
                }            
            ]
        }
    ]
}
```
<span data-ttu-id="3349e-242">Tato šablona vytvoří objekt pro vytváření dat s názvem GatewayUsingArmDF, který má bránu s názvem GatewayUsingARM.</span><span class="sxs-lookup"><span data-stu-id="3349e-242">This template creates a data factory named GatewayUsingArmDF with a gateway named: GatewayUsingARM.</span></span> 

## <a name="see-also"></a><span data-ttu-id="3349e-243">Viz také</span><span class="sxs-lookup"><span data-stu-id="3349e-243">See Also</span></span>
| <span data-ttu-id="3349e-244">Téma</span><span class="sxs-lookup"><span data-stu-id="3349e-244">Topic</span></span> | <span data-ttu-id="3349e-245">Popis</span><span class="sxs-lookup"><span data-stu-id="3349e-245">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="3349e-246">Kanály</span><span class="sxs-lookup"><span data-stu-id="3349e-246">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="3349e-247">Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku.</span><span class="sxs-lookup"><span data-stu-id="3349e-247">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="3349e-248">Datové sady</span><span class="sxs-lookup"><span data-stu-id="3349e-248">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="3349e-249">Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3349e-249">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="3349e-250">Plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="3349e-250">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="3349e-251">Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3349e-251">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="3349e-252">Monitorování a správa kanálů pomocí monitorovací aplikace</span><span class="sxs-lookup"><span data-stu-id="3349e-252">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="3349e-253">Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="3349e-253">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |

