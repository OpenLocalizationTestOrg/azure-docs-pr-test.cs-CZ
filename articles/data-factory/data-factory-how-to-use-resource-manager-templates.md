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
# <a name="use-templates-toocreate-azure-data-factory-entities"></a>Používání šablon toocreate Azure Data Factory entit
## <a name="overview"></a>Přehled
Při použití Azure Data Factory pro integraci vašim datovým potřebám sami zjistíte opakovaného použití hello stejného vzoru různých prostředích nebo implementace hello stejná úloha opakovaně v rámci hello stejné řešení. Šablony vám pomohou implementovat a spravovat tyto scénáře snadno způsobem. Šablony v Azure Data Factory jsou ideální pro scénáře, které zahrnují – opětovné použití a opakování.

Vezměte v úvahu hello situaci, kde může organizace má 10 výrobních závodech napříč hello, world. Hello protokoly z každého zařízení jsou uloženy v databázi systému SQL Server samostatný místní. Hello společnost chce toobuild jednoho datového skladu v cloudu hello ad-hoc analýzy. Je také chce toohave hello stejné logiky, která ale různé konfigurace pro vývoj, testování a provozním prostředí.

V takovém případě úlohu potřebuje toobe opakuje uvnitř hello stejné prostředí, ale s různými hodnotami napříč hello 10 datové továrny pro každého z výrobních závodů. Ve skutečnosti **opakování** je k dispozici. Ukázka umožňuje hello abstrakce tento obecný tok (tedy kanálů s hello stejné aktivity v každém objektu pro vytváření dat), ale používá soubor samostatného parametru pro každého z výrobních závodů.

Kromě toho, jak hello organizace přeje, aby toodeploy těchto 10 datové továrny vícekrát různých prostředích, šablony můžete použít **– opětovné použití** s využitím soubory samostatného parametru pro vývoj, testování, a provozní prostředí.

## <a name="templating-with-azure-resource-manager"></a>Ukázka s Azure Resource Manager
[Šablony Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#template-deployment) jsou ukázka tooachieve skvělý způsob, jak v Azure Data Factory. Šablony Resource Manageru definovat hello infrastruktury a konfigurace řešení Azure prostřednictvím soubor JSON. Protože šablon Azure Resource Manageru pracovat s všechny/většina služeb Azure, široce použitím tooeasily spravovat všechny prostředky vaše prostředky Azure. V tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) toolearn Další informace o obecně hello šablony Resource Manageru.

## <a name="tutorials"></a>Kurzy
V tématu hello následující kurzy pro podrobné pokyny entit služby Data Factory toocreate pomocí šablony Resource Manageru:

* [Kurz: Vytvoření kanálu toocopy dat pomocí šablony Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Kurz: Vytvoření kanálu tooprocess dat pomocí šablony Azure Resource Manager](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Data Factory šablony na Githubu
Podívejte se na hello následující šablony Azure rychlý start na Githubu:

* [Vytvoření objektu pro vytváření dat toocopy dat z Azure Blob Storage tooAzure databáze SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Vytvoření služby Data factory s aktivitou Hive v clusteru Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [Vytvoření objektu pro vytváření dat toocopy dat ze služby Salesforce tooAzure objektů BLOB](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Vytvoření objektu pro vytváření dat, který je zřetězený aktivity: zkopíruje data ze serveru FTP tooAzure objekty BLOB, vyvolá skript hive na na vyžádání HDInsight clusteru tootransform hello data a zkopíruje výsledek do Azure SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

Působí volné tooshare šablony Azure Data Factory v [Azure rychlý start](https://azure.microsoft.com/documentation/templates/). Odkazovat toohello [příspěvku průvodce](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) při vývoji šablony, které je možné sdílet přes toto úložiště.

Hello následující části obsahují podrobné informace o definování prostředků pro vytváření dat v šabloně Resource Manager.

## <a name="defining-data-factory-resources-in-templates"></a>Definice prostředků pro vytváření dat v šablonách
Hello nejvyšší úrovně šablonu pro definování objekt pro vytváření dat je:

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

### <a name="define-data-factory"></a>Definování datové továrny
Objekt pro vytváření dat v šabloně Resource Manager hello definovat, jak je uvedeno v hello následující ukázka:

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
Hello dataFactoryName je definováno v "proměnné" jako:

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a>Definování propojených služeb

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

V tématu [propojená služba úložiště](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [propojené výpočetní služby](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) podrobné informace o vlastnostech JSON hello hello konkrétní propojené služby chcete toodeploy. parametr "dependsOn" Hello Určuje název objektu pro vytváření dat odpovídající hello. Příklad definování propojené služby pro Azure Storage je uveden v hello následující definici JSON:

### <a name="define-datasets"></a>Definování datové sady

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
Odkazovat příliš[podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) podrobné informace o vlastnostech JSON hello pro typ konkrétní sady hello chcete toodeploy. Poznámka: hello "dependsOn" parametr určuje název odpovídajících dat hello tovární nastavení a úložiště propojené služby. Příklad definování typu datovou sadu Azure blob Storage je uveden v hello následující definici JSON:

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

### <a name="define-pipelines"></a>Definovat kanály

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

Odkazovat příliš[definování kanály](data-factory-create-pipelines.md#pipeline-json) pro informace o vlastnostech JSON hello k definování hello konkrétní kanálu a aktivity chcete toodeploy. Poznámka: hello "dependsOn" parametr určuje název objektu pro vytváření dat hello a všechny odpovídající propojené služby nebo datové sady. V následujícím fragmentu kódu JSON hello je uveden příklad kanálu, který kopíruje data z Azure Blob Storage tooAzure SQL Database:

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
## <a name="parameterizing-data-factory-template"></a>Parametrizace šablony služby Data Factory
Osvědčené postupy v Parametrizace, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) článku. Obecně platí, použití parametrů by měl být minimalizován, zejména v případě, že proměnné lze použít místo. Jenom zadejte parametry v hello následující scénáře:

* Nastavení se liší podle prostředí (Příklad: vývoj, testování a provozním)
* Tajné klíče (jako jsou hesla)

Pokud potřebujete toopull tajné klíče z [Azure Key Vault](../key-vault/key-vault-get-started.md) při nasazení entit služby Azure Data Factory pomocí šablony, zadejte hello **trezoru klíčů** a **tajný název** jak je znázorněno v Hello následující ukázka:

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
> Při exportu šablony pro existující datové továrny není aktuálně ještě podporovaná, je hello funguje.
>
>
