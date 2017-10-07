---
title: "modely Machine Learning aaaUpdate pomocí Azure Data Factory | Microsoft Docs"
description: "Popisuje, jak vytvořit toocreate prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 6e5e4d2cfd245c7a9ed3bb9cdacca1f7f82b9620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Aktualizace pomocí aktivita prostředku aktualizace modely Azure Machine Learning

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Aktivita Hive](data-factory-hive-activity.md) 
> * [Pig aktivity](data-factory-pig-activity.md)
> * [Činnost MapReduce](data-factory-map-reduce.md)
> * [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Spark aktivity](data-factory-spark.md)
> * [Aktivita Provedení dávky služby Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> * [Aktivita Aktualizace prostředků služby Machine Learning](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivita Uložená procedura](data-factory-stored-proc-activity.md)
> * [Aktivita U-SQL služby Data Lake Analytics](data-factory-usql-activity.md)
> * [Vlastní aktivity rozhraní .NET](data-factory-use-custom-activities.md)

Tento článek doplňuje hello hlavní Azure Data Factory – článek integrace Azure Machine Learning: [vytvořit prediktivní kanály pomocí Azure Machine Learning a Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md). Pokud jste tak již neučinili, přečtěte si hello hlavní článek než si přečtete prostřednictvím tohoto článku. 

## <a name="overview"></a>Přehled
V čase třeba hello prediktivní modely v experimentech vyhodnocování Azure ML hello toobe retrained pomocí nové vstupní datové sady. Po dokončení práce s retraining, budete chtít tooupdate hello vyhodnocování webové služby s hello retrained modelu ML. jsou Hello obvyklé kroky tooenable retraining a aktualizace modelů Azure ML prostřednictvím webové služby:

1. Vytvoření experimentu v [Azure ML Studio](https://studio.azureml.net).
2. Jakmile budete spokojeni s modelem hello, použít Azure ML Studio toopublish webové služby pro obě hello **výukový experiment** a vyhodnocování /**prediktivní experiment**.

Hello následující tabulka popisuje hello webové služby použité v tomto příkladu.  V tématu [Machine Learning Přeučování modelů prostřednictvím kódu programu](../machine-learning/machine-learning-retrain-models-programmatically.md) podrobnosti.

- **Cvičení webové služby** – přijímá Cvičná data a vytváří trénované modely. výstup Hello hello retraining je soubor .ilearner v Azure Blob storage. Hello **výchozí koncový bod** se automaticky vytvoří pro při publikování hello školení experimentování jako webovou službu. Můžete vytvořit další koncové body, ale hello příklad používá jenom hello výchozí koncový bod.
- **Vyhodnocování webové služby** – bez popisku dat příklady obdrží a umožňuje předpovědi. výstup Hello předpovědi může mít různé formy, například soubor .csv nebo řádků v Azure SQL database, v závislosti na konfiguraci hello hello experimentu. Hello výchozí koncový bod se automaticky vytvoří za vás, když publikujete hello prediktivní experiment jako webovou službu. 

Hello následující obrázek znázorňuje hello vztah mezi školení a vyhodnocování koncových bodů v Azure ML.

![Webové služby](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

Můžete vyvolat hello **cvičení webové služby** pomocí hello **aktivita provedení dávky Azure ML**. Vyvolání webové služby školení je stejný jako vyvolání webové služby Azure ML (vyhodnocování webová služba) pro vyhodnocování data. Hello předchozí části titulní jak tooinvoke webové služby Azure ML ze služby Azure Data Factory kanálu podrobně. 

Můžete vyvolat hello **vyhodnocování webové služby** pomocí hello **aktivita prostředku aktualizace Azure ML** tooupdate hello webové služby s nově trained model hello. Následující příklady Hello poskytují definice propojené služby: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Vyhodnocování webové služby je classic webová služba
Pokud je hello vyhodnocování webové služby **classic webové služby**, vytvořit hello druhý **jiné než výchozí a aktualizovat koncový bod** pomocí hello [portál Azure](https://manage.windowsazure.com). V tématu [vytvořit koncové body](../machine-learning/machine-learning-create-endpoint.md) najdete v článku kroky. Po vytvoření aktualizovat koncový bod hello jiné než výchozí, hello následující kroky:

* Klikněte na tlačítko **BATCH EXECUTION** tooget hello URI hodnota hello **mlEndpoint** vlastnost JSON.
* Klikněte na tlačítko **aktualizace prostředků** odkaz tooget hello URI hodnotu hello **updateResourceEndpoint** vlastnost JSON. klíč rozhraní API Hello je hello koncový bod stránky na (v pravém dolním rohu hello).

![aktualizovat koncový bod](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

Následující ukázka Hello poskytuje definici JSON ukázka hello AzureML propojené služby. Hello apiKey hello propojená služba používá pro ověřování.  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Vyhodnocování webové služby je webová služba Azure Resource Manager 
Pokud hello webové služby je nový typ hello webové služby, který zveřejňuje koncový bod Azure Resource Manager, není nutné tooadd hello druhý **jiné než výchozí** koncový bod. Hello **updateResourceEndpoint** v hello propojené služby je hello formátu: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Můžete získat hodnoty pro zástupného v adrese URL hello při dotazování hello webové služby na hello [portálu služby Azure Machine Learning webové](https://services.azureml.net/). Hello nový typ prostředku aktualizace koncového bodu vyžaduje token AAD (Azure Active Directory). Zadejte **servicePrincipalId** a **servicePrincipalKey**v AzureML propojené služby. V tématu [jak toocreate službu objektu zabezpečení a přiřaďte oprávnění toomanage prostředků Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md). Zde je ukázka definice AzureML propojené služby: 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "hello linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

Hello následujícím scénáři najdete další podrobnosti. Obsahuje příklad retraining a aktualizace Azure ML modely z kanál služby Azure Data Factory.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scénář: retraining a aktualizace model Azure ML
Tato část obsahuje ukázkový kanál, který používá hello **aktivita provedení dávky Azure ML** tooretrain modelu. kanál Hello používá také hello **aktivita prostředku aktualizace Azure ML** tooupdate hello model v hello vyhodnocování webové služby. Hello oddíl obsahuje také fragmenty kódu JSON pro všechny hello propojených služeb, datových sad a kanálu v příkladu hello.

Zde je zobrazení diagramu hello hello ukázkový kanál. Jak můžete vidět, hello aktivita provedení dávky Azure ML přijímá hello školení vstup a výstup školení (soubor iLearner). Hello aktivita prostředku aktualizace Azure ML trvá tento výstup školení a aktualizace hello model v hello vyhodnocování koncový bod webové služby. Hello aktivita prostředku aktualizace nevytváří žádný výstup. Hello placeholderBlob je právě fiktivní výstupní datovou sadu, která vyžadují hello Azure Data Factory služby toorun hello kanálu.

![diagram kanálu](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Propojená služba Azure Blob storage:
Hello Azure Storage obsahuje hello následující data:

* Cvičná data. Hello vstupní data pro hello Azure ML školení webové služby.  
* reprezentuje soubor iLearner. Hello výstup z hello Azure ML školení webové služby. Tento soubor je také hello vstupní toohello aktivita prostředku aktualizace.  

Tady je definici JSON ukázka hello hello propojené služby:

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a>Školení vstupní datové sady:
Hello následující datovou sadu představuje hello školení vstupní data pro hello Azure ML školení webovou službu. Hello aktivita provedení dávky Azure ML trvá tuto datovou sadu jako vstup.

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "policy": {          
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

### <a name="training-output-dataset"></a>Školení výstupní datovou sadu:
Hello následující datovou sadu reprezentuje soubor iLearner výstup hello z hello Azure ML školení webové služby. Hello aktivita provedení dávky Azure ML vyprodukuje tuto datovou sadu. Tato datová sada je také hello vstupní toohello aktivita prostředku aktualizace Azure ML.

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a>Propojené služby pro koncový bod Azure ML školení
Následující fragment kódu JSON Hello definuje službou Azure Machine Learning, která je propojená, která ukazuje toohello výchozí koncový bod hello školení webové služby.

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

V **Azure ML Studio**, hello tyto hodnoty tooget pro **mlEndpoint** a **apiKey**:

1. Klikněte na tlačítko **webové služby** v levé nabídce hello.
2. Klikněte na tlačítko hello **cvičení webové služby** v seznamu hello webových služeb.
3. Klikněte na tlačítko Kopírovat další příliš**klíč rozhraní API** textové pole. Vložte klíč hello hello schránky do editoru JSON objektu pro vytváření dat hello.
4. V hello **Azure ML studio**, klikněte na tlačítko **BATCH EXECUTION** odkaz.
5. Kopírování hello **URI požadavku** z hello **požadavku** části a vložte ho do editoru JSON objektu pro vytváření dat hello.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Propojená služba Azure ML aktualizovat vyhodnocování koncového bodu:
Následující fragment kódu JSON Hello definuje služby Azure Machine Learning propojené, který odkazuje toohello jiné než výchozí aktualizovat koncový bod hello vyhodnocování webové služby.  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a>Zástupný symbol výstupní datovou sadu:
Hello aktivita prostředku aktualizace Azure ML negeneruje žádný výstup. Azure Data Factory však vyžaduje plán výstupní datová sada toodrive hello kanálu. Proto jsme použít datovou sadu fiktivní/zástupný symbol v tomto příkladu.  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a>Kanál
Hello kanálu má dvě aktivity: **AzureMLBatchExecution** a **AzureMLUpdateResource**. Hello aktivita provedení dávky Azure ML trvá hello Cvičná data jako vstup a vytvoří soubor iLearner jako výstup. Aktivita Hello vyvolá hello školení webové služby (výukový experiment zveřejněné jako webovou službu) se vstupem hello trénovací data a obdrží od hello webservice soubor ilearner hello. Hello placeholderBlob je právě fiktivní výstupní datovou sadu, která vyžadují hello Azure Data Factory služby toorun hello kanálu.

![diagram kanálu](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
