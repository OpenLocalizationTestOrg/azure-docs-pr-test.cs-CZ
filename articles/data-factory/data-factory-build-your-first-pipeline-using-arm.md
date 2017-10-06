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
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí šablony Azure Resource Manageru
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
> 
> 

V tomto článku je použít toocreate šablony Azure Resource Manager první objekt pro vytváření dat Azure. kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu.

Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**. Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data. Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas. 

> [!NOTE]
> Hello datovém kanálu v tomto kurzu transformuje vstupní data tooproduce výstupní data. Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Hello kanálu v tomto kurzu má jenom jedna aktivita typu: HDInsightHive. Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="prerequisites"></a>Požadavky
* Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.
* Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku tooinstall nejnovější verzi prostředí Azure PowerShell ve vašem počítači.
* V tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) toolearn o šablonách Azure Resource Manager. 

## <a name="in-this-tutorial"></a>V tomto kurzu
| Entita | Popis |
| --- | --- |
| Propojená služba Azure Storage |Propojí datovou továrnu toohello účet úložiště Azure. blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce. |
| Propojená služba HDInsightu na vyžádání |Propojení služby na vyžádání HDInsight clusteru toohello data factory. Hello clusteru se automaticky vytvoří za vás tooprocess dat a odstraní se po dokončení zpracování hello. |
| Vstupní datová sada Azure Blob |Odkazuje toohello propojenou službu úložiště Azure. Hello propojená služba odkazuje tooan účtu Azure Storage a datové sady objektu Blob Azure hello určuje hello kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní data hello. |
| Výstupní datová sada Azure Blob |Odkazuje toohello propojenou službu úložiště Azure. Hello propojená služba odkazuje tooan účtu Azure Storage a datové sady objektu Blob Azure hello určuje hello kontejner, složku a název souboru v úložišti hello, který obsahuje hello výstupní data. |
| Data Pipeline |Hello kanálu má jednu aktivitu typu HDInsightHive, který využívá hello vstupní datové sady a vytváří hello výstupní datovou sadu. |

Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Existují dva typy aktivit: [aktivity přesunu dat](data-factory-data-movement-activities.md) a [transformace dat](data-factory-data-transformation-activities.md). V tomto kurzu vytvoříte kanál s jednou aktivitou (aktivita Hive).

Hello následující část obsahuje hello dokončení šablony Resource Manageru pro definování entit služby Data Factory, takže můžete rychle spustit prostřednictvím hello kurzu a testování hello šablony. toounderstand o každé entity služby Data Factory je definován, v tématu [entit služby Data Factory v šabloně hello](#data-factory-entities-in-the-template) části.

## <a name="data-factory-json-template"></a>Šablona JSON služby Data Factory
Hello nejvyšší úrovně šablony Resource Manageru pro definování objekt pro vytváření dat je: 

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
Vytvořte soubor JSON s názvem **C:\adfgetstarted** v **C:\ADFGetStarted** složku s hello následující obsah:

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
> Další příklad šablony Resource Manageru pro vytváření datové továrny Azure najdete v části [Kurz: Vytvoření kanálu s aktivitou kopírování pomocí šablony Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  
> 
> 

## <a name="parameters-json"></a>Parametry JSON
Vytvořte soubor JSON s názvem **ADFTutorialARM Parameters.JSON tímto kódem** obsahující parametry šablony Azure Resource Manager hello.  

> [!IMPORTANT]
> Zadejte název hello a klíč účtu úložiště Azure pro hello **storageAccountName** a **storageAccountKey** parametry v tomto souboru parametrů. 
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
> Může mít soubory JSON samostatného parametru pro vývoj, testování a provozním prostředí, která můžete použít s hello stejné šablony Data Factory JSON. Pomocí skriptu prostředí PowerShell můžete zautomatizovat nasazování entit služby Data Factory v těchto prostředích. 
> 
> 

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
1. Spustit **prostředí Azure PowerShell** a hello spusťte následující příkaz: 
   * Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.
    ```PowerShell
    Login-AzureRmAccount
    ```  
   * Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.
    ```PowerShell
    Get-AzureRmSubscription
    ``` 
   * Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello. Toto předplatné by měl být hello stejné jako hello jeden, které jste použili v hello portálu Azure.
    ```
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```   
2. Spusťte hello následující entity služby Data Factory toodeploy příkaz pomocí šablony Resource Manageru hello, kterou jste vytvořili v kroku 1. 

    ```PowerShell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Monitorování kanálu
1. Po přihlášení toohello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet** a vyberte **datové továrny**.
     ![Procházet -&gt; Datové továrny](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2. V hello **datové továrny** okně klikněte na objekt pro vytváření dat hello (**TutorialFactoryARM**) jste vytvořili.    
3. V hello **Data Factory** objektu pro vytváření dat, klikněte na **Diagram**.

     ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4. V hello **zobrazení diagramu**, zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.
   
   ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
5. V hello zobrazení diagramu, klikněte dvakrát na datovou sadu hello **AzureBlobOutput**. Uvidíte, že hello řez, který se právě zpracovává.
   
    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
6. Po dokončení zpracování uvidíte hello řez ve **připraven** stavu. Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut). Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.
   
    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png)    
7. Pokud je řez hello v **připraven** stavu, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.  

V tématu [monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md) pokyny, jak toouse hello oken Azure portal toomonitor hello kanálu a datových sad jste vytvořili v tomto kurzu.

Můžete také použít monitorování a správě aplikací toomonitor datových kanálů. V tématu [monitorování a Správa kanálů služby Azure Data Factory pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) podrobnosti o použití aplikace hello. 

> [!IMPORTANT]
> vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní. Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.
> 
> 

## <a name="data-factory-entities-in-hello-template"></a>Entity objektu pro vytváření dat v šabloně hello
### <a name="define-data-factory"></a>Definování datové továrny
Objekt pro vytváření dat v šabloně Resource Manager hello definovat, jak je uvedeno v hello následující ukázka:  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```
Hello dataFactoryName je definován jako: 

```json
"dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
```
Jedná se o jedinečný řetězec na základě ID hello prostředků skupiny.  

### <a name="defining-data-factory-entities"></a>Definování entit služby Data Factory
Hello následující entity služby Data Factory jsou definovány v šabloně hello JSON: 

* [Propojená služba Azure Storage](#azure-storage-linked-service)
* [Propojená služba HDInsightu na vyžádání](#hdinsight-on-demand-linked-service)
* [Vstupní datová sada Azure Blob](#azure-blob-input-dataset)
* [Výstupní datová sada Azure Blob](#azure-blob-output-dataset)
* [Data Pipeline s aktivitou kopírování](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
Zadejte název hello a klíč účtu úložiště Azure v této části. V tématu [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby. 

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
Hello **connectionString** používá hello storageAccountName a storageAccountKey parametry. Hello hodnoty pro tyto parametry předané pomocí konfiguračního souboru. definice Hello také používá proměnné: azureStroageLinkedService a dataFactoryName definovaná v šabloně hello. 

#### <a name="hdinsight-on-demand-linked-service"></a>Propojená služba HDInsightu na vyžádání
V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) článek podrobnosti o toodefine vlastnosti používané JSON propojené služby HDInsight na vyžádání.  

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
Všimněte si hello následující body: 

* Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello výše JSON. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
* Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**. Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
* Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. S na vyžádání propojené služby HDInsight, HDInsight cluster vytvoří pokaždé, když řez musí toobe zpracování, pokud neexistuje aktivní cluster (**timeToLive**) a po dokončení zpracování hello, se odstraní.
  
    Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času". Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.

Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).

#### <a name="azure-blob-input-dataset"></a>Vstupní datová sada Azure Blob
Zadáte názvy hello kontejner objektů blob, složku a soubor, který obsahuje vstupní data hello. V tématu [vlastnosti datové sady objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob. 

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
Tato definice používá hello následující parametry definované v parametru šablony: blobContainer, inputBlobFolder a inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Výstupní datová sada Azure Blob
Zadáte názvy hello kontejner objektů blob a složky, která obsahuje hello výstupní data. V tématu [vlastnosti datové sady objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.  

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

Tato definice používá následující parametry definované v šabloně parametr hello hello: blobContainer a outputBlobFolder. 

#### <a name="data-pipeline"></a>Data Pipeline
Definujete kanál, který převádí data spuštěním skriptu Hive v clusteru Azure HDInsight na vyžádání. V tématu [JSON kanálu](data-factory-create-pipelines.md#pipeline-json) popisy toodefine prvky používané JSON kanálu v tomto příkladu. 

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

## <a name="reuse-hello-template"></a>Opakované použití hello šablony
V kurzu hello jste vytvořili šablonu pro definování entit služby Data Factory a šablonu pro předávání hodnot pro parametry. toouse hello stejné šablony toodeploy objekt pro vytváření dat entity toodifferent prostředí, můžete vytvořit soubor parametrů pro každé prostředí a použít je při nasazování toothat prostředí.     

Příklad:  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```
Všimněte si, že hello první příkaz používá parametr souboru hello vývojového prostředí, druhá pro hello testovací prostředí a hello třetí jeden hello produkční prostředí.  

Také můžete opakovaně použít hello šablony tooperform opakující úlohy. Například musíte toocreate mnoho objektů pro vytváření dat s jednou nebo více kanálů, které implementují hello stejné logiky, ale každý data factory používá jiný úložiště Azure a účty Azure SQL Database. V tomto scénáři použijete hello stejné šablony v hello stejné prostředí (vývojářů, testovací nebo produkční) s jiným parametrem soubory toocreate datové továrny. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Šablona Resource Manageru pro vytvoření brány
Zde je ukázka šablony Resource Manageru pro vytvoření logické brány v hello zpět. Nainstalujte bránu do svého místního počítače nebo virtuálního počítače Azure IaaS a zaregistrujte bránu hello službě Data Factory pomocí klíče. Podrobnosti najdete v článku [Přesun dat mezi místním prostředím a cloudem](data-factory-move-data-between-onprem-and-cloud.md).

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
Tato šablona vytvoří objekt pro vytváření dat s názvem GatewayUsingArmDF, který má bránu s názvem GatewayUsingARM. 

## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací. |

