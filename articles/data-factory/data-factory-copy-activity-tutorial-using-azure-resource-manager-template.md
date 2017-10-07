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
# <a name="tutorial-use-azure-resource-manager-template-toocreate-a-data-factory-pipeline-toocopy-data"></a>Kurz: Správce prostředků Azure pomocí šablony toocreate toocopy datový kanál služby Data Factory 
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Tento kurz ukazuje, jak toouse toocreate šablony Azure Resource Manager služby Azure data factory. Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Ho nebudou transformovat vstupní data tooproduce výstupní data. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).

V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování. Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště. Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).

Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md). 

## <a name="prerequisites"></a>Požadavky
* Projděte [přehled kurzu a požadované součásti](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a dokončení hello **požadavek** kroky.
* Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku tooinstall nejnovější verzi prostředí Azure PowerShell ve vašem počítači. V tomto kurzu použijete entit služby Data Factory toodeploy prostředí PowerShell. 
* (volitelné) V tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) toolearn o šablonách Azure Resource Manager.

## <a name="in-this-tutorial"></a>V tomto kurzu
V tomto kurzu vytvoříte objekt pro vytváření dat s hello následující entity služby Data Factory:

| Entita | Popis |
| --- | --- |
| Propojená služba Azure Storage |Propojí datovou továrnu toohello účet úložiště Azure. Azure Storage je hello zdrojového úložiště dat a Azure SQL database je hello podřízený datové úložiště pro aktivitu kopírování hello v kurzu hello. Určuje účet úložiště hello, který obsahuje hello vstupní data pro aktivitu kopírování hello. |
| Propojená služba Azure SQL Database |Propojí toohello datovou továrnu Azure SQL database. Určuje hello Azure SQL database, která obsahuje hello výstupní data pro aktivitu kopírování hello. |
| Vstupní datová sada Azure Blob |Odkazuje toohello propojenou službu úložiště Azure. Hello propojená služba odkazuje tooan účtu Azure Storage a datové sady objektu Blob Azure hello určuje hello kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní data hello. |
| Výstupní datová sada Azure SQL |Odkazuje toohello propojená služba Azure SQL. Hello propojená služba Azure SQL odkazuje tooan server Azure SQL a Azure SQL hello datovou sadu Určuje název hello hello tabulky, který obsahuje hello výstupní data. |
| Data Pipeline |Hello kanálu má jednu aktivitu zadejte kopírování, která přebírá hello datovou sadu objektů blob v Azure jako vstup a hello Azure SQL datovou sadu jako výstup. Aktivita kopírování Hello kopíruje data ze objektů blob v Azure tooa tabulky v databázi Azure SQL hello. |

Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Existují dva typy aktivit: [aktivity přesunu dat](data-factory-data-movement-activities.md) a [transformace dat](data-factory-data-transformation-activities.md). V tomto kurzu vytvoříte kanál s jednou aktivitou (aktivita kopírování).

![Zkopírujte tooAzure objektů Blob v Azure SQL Database](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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
Vytvořte soubor JSON s názvem **ADFCopyTutorialARM.json** v **C:\ADFGetStarted** složku s hello následující obsah:

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

## <a name="parameters-json"></a>Parametry JSON
Vytvořte soubor JSON s názvem **ADFCopyTutorialARM Parameters.JSON tímto kódem** obsahující parametry šablony Azure Resource Manager hello. 

> [!IMPORTANT]
> Zadejte název a klíč svého účtu služby Azure Storage v parametrech storageAccountName a storageAccountKey.  
> 
> Zadejte server SQL Azure, databázi, uživatele a heslo v parametrech sqlServerName, databaseName, sqlServerUserName, a sqlServerPassword.  

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
   * Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. Spusťte hello následující entity služby Data Factory toodeploy příkaz pomocí šablony Resource Manageru hello, kterou jste vytvořili v kroku 1.

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Monitorování kanálu

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu Azure.
2. Klikněte na tlačítko **datové továrny** v hello levé nabídce (nebo) klikněte na tlačítko **další služby** a klikněte na tlačítko **datové továrny** pod **INTELLIGENCE + analýzy** kategorie.
   
    ![Nabídka Datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. V hello **datové továrny** stránky, Hledat a najít služby data factory (AzureBlobToAzureSQLDatabaseDF). 
   
    ![Vyhledávání datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klikněte na svou datovou továrnu Azure. Zobrazí hello Domovská stránka objektu pro vytváření dat hello.
   
    ![Domovská stránka datové továrny](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. Postupujte podle pokynů v [monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello kanálu a datových sad v tomto kurzu jste vytvořili. V současné době Visual Studio monitorování kanálů Data Factory nepodporuje.
7. Pokud je řez v hello **připraven** stavu, ověřte, že hello data zkopírovaný toohello **emp** tabulky v databázi Azure SQL hello.


Další informace o tom, jak toouse oken Azure portal toomonitor kanálu a datových sad jste vytvořili v tomto kurzu, najdete v části [monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md) .

Další informace o tom, jak toouse hello monitorování a Správa aplikací toomonitor data kanálů najdete v tématu [monitorování a Správa kanálů služby Azure Data Factory pomocí monitorovací aplikace](data-factory-monitor-manage-app.md).

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
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

Jedná se o jedinečný řetězec na základě ID hello prostředků skupiny.  

### <a name="defining-data-factory-entities"></a>Definování entit služby Data Factory
Hello následující entity služby Data Factory jsou definovány v šabloně hello JSON: 

1. [Propojená služba Azure Storage](#azure-storage-linked-service)
2. [Propojená služba Azure SQL](#azure-sql-database-linked-service)
3. [Datová sada Azure Blob](#azure-blob-dataset)
4. [Datová sada Azure SQL](#azure-sql-dataset)
5. [Data Pipeline s aktivitou kopírování](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Vytvořit kontejner a účet úložiště dat toothis odesílané jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Zadejte název hello a klíč účtu úložiště Azure v této části. V tématu [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby. 

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

Hello connectionString používá hello storageAccountName a storageAccountKey parametry. Hello hodnoty pro tyto parametry předané pomocí konfiguračního souboru. definice Hello také používá proměnné: azureStroageLinkedService a dataFactoryName definovaná v šabloně hello. 

#### <a name="azure-sql-database-linked-service"></a>Propojená služba Azure SQL Database
AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Zadejte název serveru Azure SQL hello, název databáze, uživatelské jméno a heslo uživatele v této části. V tématu [propojená služba Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) podrobnosti o toodefine vlastnosti používané JSON Azure SQL propojené služby.  

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

Hello connectionString používá sqlServerName, databaseName, sqlServerUserName a sqlServerPassword parametry, jejichž hodnoty se předávají pomocí konfiguračního souboru. Hello definice také používá následující proměnné z šablony hello hello: azureSqlLinkedServiceName dataFactoryName.

#### <a name="azure-blob-dataset"></a>Datová sada Azure Blob
Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. V definici datové sady objektu blob Azure zadejte názvy kontejneru objektů blob, složku a soubor, který obsahuje vstupní data hello. V tématu [vlastnosti datové sady objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob. 

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

#### <a name="azure-sql-dataset"></a>Datová sada Azure SQL
Zadáte název hello hello tabulky v databázi Azure SQL hello, která obsahuje hello zkopírovat data z hello úložiště objektů Blob v Azure. V tématu [vlastnosti datové sady Azure SQL](data-factory-azure-sql-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure SQL. 

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

#### <a name="data-pipeline"></a>Data Pipeline
Můžete definovat kanál, který kopíruje data z datové sady hello objektů blob v Azure datovou sadu toohello Azure SQL. V tématu [JSON kanálu](data-factory-create-pipelines.md#pipeline-json) popisy toodefine prvky používané JSON kanálu v tomto příkladu. 

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

## <a name="reuse-hello-template"></a>Opakované použití hello šablony
V kurzu hello jste vytvořili šablonu pro definování entit služby Data Factory a šablonu pro předávání hodnot pro parametry. Hello kanál kopíruje data z databáze Azure SQL tooan účtu Azure Storage zadaný prostřednictvím parametrů. toouse hello stejné šablony toodeploy objekt pro vytváření dat entity toodifferent prostředí, můžete vytvořit soubor parametrů pro každé prostředí a použít je při nasazování toothat prostředí.     

Příklad:  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

Všimněte si, že hello první příkaz používá parametr souboru hello vývojového prostředí, druhá pro hello testovací prostředí a hello třetí jeden hello produkční prostředí.  

Také můžete opakovaně použít hello šablony tooperform opakující úlohy. Například musíte toocreate mnoho objektů pro vytváření dat s jednou nebo více kanálů, které implementují hello stejné logiky, ale každý objekt pro vytváření dat používá různé účty úložiště a databáze SQL. V tomto scénáři použijete hello stejné šablony v hello stejné prostředí (vývojářů, testovací nebo produkční) s jiným parametrem soubory toocreate datové továrny.   

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.
