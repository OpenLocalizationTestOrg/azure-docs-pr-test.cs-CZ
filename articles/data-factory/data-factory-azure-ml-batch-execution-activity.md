---
title: "Prediktivní datových kanálů aaaCreate pomocí Azure Data Factory | Microsoft Docs"
description: "Popisuje, jak vytvořit toocreate prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 943210c28b1696e299ff9b7cc96369b95f182354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Vytvořit prediktivní kanály pomocí Azure Machine Learning a Azure Data Factory

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

## <a name="introduction"></a>Úvod

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) umožňuje toobuild můžete otestovat a nasadit řešení prediktivní analýzy. Z vysoké úrovně hlediska se provádí v tři kroky:

1. **Vytvoření experimentu školení**. Tento krok se provádí pomocí hello Azure ML Studio. Hello ML studio je spolupráce vizuální vývojové prostředí použít tootrain a testování pomocí Cvičná data modelu prediktivní analýzy.
2. **Převést prediktivní experiment tooa**. Jakmile se v modelu má cvičena s existujícími daty a jsou připravené toouse ho tooscore nová data, připravte a zjednodušit experimentu pro vyhodnocování.
3. **Nasadit jako webovou službu**. Vyhodnocování experimentu můžete publikovat jako Azure webové služby. Můžete odesílat datový model tooyour přes tento koncový bod webové služby a přijímat výsledek předpovědi u tabulátorů hello modelu.  

### <a name="azure-data-factory"></a>Azure Data Factory
Data Factory je Cloudová datová integrační služba, která orchestruje a automatizuje hello **přesun** a **transformace** dat. Můžete vytvořit integrace řešení pro data pomocí Azure Data Factory můžete načítání dat z různých úložišť dat, které proces nebo transformace dat hello nebo publikování hello výsledek data toohello datová úložiště.

Služba data Factory můžete toocreate datových kanálů, které přesunout a transformovat data a spusťte hello kanálů podle zadaného plánu (HODINOVĚ, denně, týdně, atd.). Také poskytuje bohatých vizualizací toodisplay hello rodokmenu a závislostí mezi vlastními kanály a monitorování všech datových kanálů z přesně stanovit problémů jednoho jednotného zobrazení tooeasily a nastavit upozornění monitorování.

V tématu [Úvod tooAzure Data Factory](data-factory-introduction.md) a [sestavit svůj první kanál](data-factory-build-your-first-pipeline.md) články tooquickly začít pracovat s hello služby Azure Data Factory.

### <a name="data-factory-and-machine-learning-together"></a>Objekt pro vytváření dat a strojové učení společně
Azure Data Factory umožňuje tooeasily můžete vytvořit kanály, které používají publikovaný [Azure Machine Learning] [ azure-machine-learning] webová služba pro prediktivní analýzy. Pomocí hello **aktivita provedení dávky** v kanál služby Azure Data Factory můžete vyvolat Azure ML web service toomake předpovědi hello data v dávce. V tématu [Azure ML vyvolání webové služby pomocí hello aktivita provedení dávky](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) podrobnosti.

V čase třeba hello prediktivní modely v experimentech vyhodnocování Azure ML hello toobe retrained pomocí nové vstupní datové sady. Model Azure ML z objektu pro vytváření dat kanál můžete přeučování díky hello následující kroky:

1. Publikujte hello výukový experiment (ne prediktivní experiment) jako webovou službu. Tento krok v hello Azure ML Studio můžete udělat, jako jste to udělali prediktivní experiment tooexpose jako webovou službu v předchozím scénáři hello.
2. Použijte hello aktivita provedení dávky Azure ML tooinvoke hello webovou službu pro hello výukový experiment. V podstatě můžete použít tooinvoke aktivita provedení dávky Azure ML hello cvičení webová služba i vyhodnocování webové služby.

Po dokončení práce s retraining, aktualizujte hello vyhodnocování webové služby (zveřejněné jako webové služby prediktivní experiment) s nově trained model hello pomocí hello **aktivita prostředku aktualizace Azure ML**. V tématu [aktualizace modelů pomocí aktivita prostředku aktualizace](data-factory-azure-ml-update-resource-activity.md) článku.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Vyvolání webové služby pomocí aktivita provedení dávky
Použijte přesun dat tooorchestrate Azure Data Factory a zpracování a poté proveďte dávkového spuštění pomocí Azure Machine Learning. Zde jsou kroky nejvyšší úrovně hello:

1. Vytvoření služby Azure Machine Learning propojený. Je třeba hello následující hodnoty:

   1. **Identifikátor URI požadavku je** pro hello Batch provádění API. Můžete najít hello URI požadavku kliknutím hello **BATCH EXECUTION** odkaz hello webové služby stránce.
   2. **Klíč rozhraní API** hello publikovaného webovou službu Azure Machine Learning. Klíč rozhraní API hello můžete získat kliknutím hello webové službě, kterou jste publikovali.
   3. Použití hello **AzureMLBatchExecution** aktivity.

      ![Strojového učení řídicí panel](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch identifikátor URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-toodata-in-azure-blob-storage"></a>Scénář: Experimenty pomocí webové služby vstupy/výstupy odkazovala toodata ve službě Azure Blob Storage
V tomto scénáři hello Azure Machine Learning webové služby umožňuje předpovědi pomocí dat ze souboru v Azure blob storage a ukládá výsledky předpovědi hello v úložišti objektů blob hello. Hello následující kód JSON určuje objekt pro vytváření dat kanál s AzureMLBatchExecution aktivitu. Aktivita Hello má datovou sadu hello **DecisionTreeInputBlob** jako vstup a **DecisionTreeResultBlob** jako výstup hello. Hello **DecisionTreeInputBlob** se předá jako webové služby vstupní toohello podle pomocí hello **webServiceInput** vlastnost JSON. Hello **DecisionTreeResultBlob** se předá jako výstup toohello webové služby s použitím hello **webServiceOutputs** vlastnost JSON.  

> [!IMPORTANT]
> Pokud hello webová služba přijímá více vstupů, použijte hello **webServiceInputs** vlastnost místo použití **webServiceInput**. V tématu hello [webová služba vyžaduje více vstupů](#web-service-requires-multiple-inputs) části příklad pomocí vlastnosti webServiceInputs hello.
>
> Datové sady, které odkazují hello **webServiceInput**/**webServiceInputs** a **webServiceOutputs** vlastnosti (v  **rámci typeProperties**) musí být i součástí hello aktivity **vstupy** a **výstupy**.
>
> V experimentu Azure ML vstup webové služby a porty výstup a globální parametry mají výchozí názvy ("input1", "input2"), které můžete přizpůsobit. názvy Hello, který použijete pro webServiceInputs, webServiceOutputs a globalParameters nastavení musí přesně shodovat hello názvy v experimentech hello. Datová část požadavku ukázka hello můžete zobrazit na stránce nápovědy spuštění dávky hello pro vaši Azure ML koncový bod tooverify hello očekává mapování.
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> Pouze vstupy a výstupy hello AzureMLBatchExecution aktivity mohou být předána jako toohello parametry webové služby. Například v hello výše fragmentu kódu JSON, je DecisionTreeInputBlob vstupní toohello AzureMLBatchExecution aktivity, které se předá jako vstupní toohello webové služby pomocí parametru webServiceInput.   
>
>

### <a name="example"></a>Příklad
Tento příklad používá úložiště Azure toohold obě hello vstupní a výstupní data.

Doporučujeme projít hello [sestavit svůj první kanál pomocí služby Data Factory] [ adf-build-1st-pipeline] kurz před projít tento příklad. V tomto příkladu použijte hello editoru služby Data Factory toocreate artefaktů služby Data Factory (propojené služby, datové sady, kanál).   

1. Vytvoření **propojená služba** pro vaše **Azure Storage**. Pokud hello vstupní a výstupní soubory jsou v jiným účtům úložiště, je třeba dvě propojené služby. Tady je příklad JSON:

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. Vytvoření hello **vstupní** Azure Data Factory **datovou sadu**. Na rozdíl od některých jiných objekt pro vytváření dat datové sady, musí obsahovat tyto datové sady obě **folderPath** a **fileName** hodnoty. Můžete použít rozdělení toocause tooprocess každé dávky spuštění (každý datový řez) nebo vytvořit jedinečný vstupní a výstupní soubory. Může být nutné tooinclude některé hello tootransform nadřízené činnosti vstup na formát souboru CSV hello a umístěte jej v hello účet úložiště pro každý řez. V takovém případě by zahrnovat hello **externí** a **externalData** nastavení zobrazené v hello následující příklad a vaše DecisionTreeInputBlob by hello výstupní datovou sadu jiné aktivitě.

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
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

    Soubor vstupní csv musí obsahovat hello řádek záhlaví sloupců. Pokud používáte hello **aktivity kopírování** csv hello toocreate nebo přesunout do úložiště objektů blob hello, měli byste nastavit vlastnost jímky hello **blobWriterAddHeader** příliš**true**. Například:

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    Pokud soubor csv hello nemá hello řádek záhlaví, mohou se zobrazit hello následující chyba: **Chyba v aktivitě: Chyba při čtení řetězec. Neočekávaný token: StartObject. Cesta %, řádek 1, 1 umístit**.
3. Vytvoření hello **výstup** Azure Data Factory **datovou sadu**. Tento příklad používá rozdělení toocreate jedinečný výstupní cesta pro každé spuštění řezu. Bez hello oddílů, by hello aktivity přepsat soubor hello.

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. Vytvoření **propojená služba** typu: **AzureMLLinkedService**, poskytuje klíč hello rozhraní API a modelu adresa URL pro spuštění dávky.

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. Nakonec vytvořte kanálu obsahující **AzureMLBatchExecution** aktivity. V době běhu provádí kanálu hello následující kroky:

   1. Získá umístění hello hello vstupní soubor z vaší vstupní datové sady.
   2. Vyvolá hello Azure Machine Learning dávkového spuštění rozhraní API
   3. Kopie hello batch provádění výstup toohello objektů blob v výstupní datovou sadu.

      > [!NOTE]
      > Aktivita AzureMLBatchExecution může mít v počtu nula či více vstupů a výstupů jeden nebo více.
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      Obě **spustit** a **end** času musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2014-10-14T16:32:41Z. Hello **end** čas je volitelné. Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin.**" bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost. Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referenční příručka skriptování JSON).

      > [!NOTE]
      > Určení vstup aktivity AzureMLBatchExecution hello je volitelné.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-toorefer-toodata-in-various-storages"></a>Scénář: Experimenty v různých úložných toodata toorefer moduly Reader/Writer
Další z typických možností při vytváření experimenty Azure ML je moduly toouse čtení a zápis. modul čtečky Hello je použité tooload data do experimentu a modul zapisovače hello je toosave data z experimentů. Podrobnosti o čtení a zápis modulech najdete v tématu [čtečky](https://msdn.microsoft.com/library/azure/dn905997.aspx) a [zapisovače](https://msdn.microsoft.com/library/azure/dn905984.aspx) témata v knihovně MSDN.     

Pokud používáte moduly hello čtení a zápis, je vhodné toouse parametr webové služby pro každou vlastnost tyto moduly čtečky nebo zapisovač. Tyto parametry webové povolit hodnoty hello tooconfigure za běhu. Můžete například vytvořit experimentu s čtečky modul, který používá Azure SQL Database: XXX.database.windows.net. Po nasazení webové služby hello chcete tooenable hello spotřebiteli hello webové služby toospecify názvem YYY.database.windows.net jiný Server SQL Azure. Můžete vytvořit webové služby parametr tooallow tuto hodnotu toobe nakonfigurované.

> [!NOTE]
> Webové služby vstup a výstup se liší od parametry webové služby. V hello prvního scénáře jste viděli, jak vstup a výstup lze zadat pro Azure ML webové služby. V tomto scénáři můžete předat parametry webové služby, které odpovídají tooproperties modulů čtečky nebo zapisovač.
>
>

Podívejme se na scénář použití parametry webové služby. Máte nasazené webové službě Azure Machine Learning, která používá čtečky modulu tooread data z jednoho z hello zdroje dat podporované rozhraním Azure Machine Learning (například: Azure SQL Database). Po provedení dávkového spuštění hello hello výsledky jsou zapsány pomocí zapisovače modulu (Azure SQL Database).  Žádné webové služby vstupy a výstupy jsou definovány v hello experimenty. V takovém případě doporučujeme nakonfigurovat parametry relevantní webové služby pro čtení a zápis moduly hello. Tato konfigurace umožňuje hello čtečky nebo zapisovač toobe moduly, které jsou nakonfigurované při použití hello AzureMLBatchExecution aktivity. Zadejte parametry webové služby v hello **globalParameters** část v kódu JSON aktivity hello následujícím způsobem.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

Můžete také použít [funkce objektu pro vytváření dat](data-factory-functions-variables.md) v předávání hodnot pro hello parametry webové služby, jak je znázorněno v hello následující ukázka:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> parametry webové služby Hello malá a velká písmena, zajistěte proto, že hello názvy, které zadáte v aktivitě hello JSON odpovídají hello ty, které jsou vystavené hello webové služby.
>
>

### <a name="using-a-reader-module-tooread-data-from-multiple-files-in-azure-blob"></a>Pomocí čtečky modulu tooread dat z více souborů v Azure Blob
Velké objemy dat kanálů s aktivity, například Pig a Hive můžete vytvořit jeden nebo více výstupních souborů bez přípony. Například když zadáte externí tabulku Hive, hello data pro externí tabulku Hive hello mohou být uložena v úložišti objektů Azure blob s následující název 000000_0 hello. Můžete použít modul čtečky hello v tooread experimentu několik souborů a použít je k předpovědi.

Pokud používáte modul čtečky hello v experimentu Azure Machine Learning, můžete zadat objektů Blob v Azure jako vstup. soubory Hello v hello úložiště objektů blob Azure může být hello výstupní soubory (Příklad: 000000_0), vznikají pomocí funkcí Pig a Hive skript spuštěný v HDInsight. Hello modul čtečky vám umožní tooread souborů (žádné) tak, že nakonfigurujete hello **toocontainer cestu, adresáře nebo objekt blob**. Hello **cesta toocontainer** body toohello kontejneru a **adresáře nebo objekt blob** body toofolder, který obsahuje soubory hello, jak ukazuje následující obrázek hello. Hello se tedy hvězdička \*) **Určuje, že všechny soubory v kontejneru nebo složku, hello hello (to znamená, data, aggregateddata a rok 2014/měsíc-6 = /\*)** se čtou v rámci experimentu hello.

![Azure Blob vlastnosti](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Příklad
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Kanál s aktivitou AzureMLBatchExecution s parametry webové služby

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

V hello výše příklad JSON:

* Hello nasadit Azure Machine Learning webové služby používá čtečka čipových karet a zapisovače modulu tooread a zápis dat z / tooan Azure SQL Database. Tato webová služba zpřístupní hello následující čtyři parametry: název serveru, název databáze, název serveru uživatelského účtu a heslo uživatelského účtu serveru databáze.  
* Obě **spustit** a **end** času musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2014-10-14T16:32:41Z. Hello **end** čas je volitelné. Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin.**" bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost. Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referenční příručka skriptování JSON).

### <a name="other-scenarios"></a>Další scénáře
#### <a name="web-service-requires-multiple-inputs"></a>Webová služba vyžaduje více vstupů
Pokud hello webová služba přijímá více vstupů, použijte hello **webServiceInputs** vlastnost místo použití **webServiceInput**. Datové sady, které odkazují hello **webServiceInputs** musí také obsahovat hello aktivity **vstupy**.

V experimentu Azure ML vstup webové služby a porty výstup a globální parametry mají výchozí názvy ("input1", "input2"), které můžete přizpůsobit. názvy Hello, který použijete pro webServiceInputs, webServiceOutputs a globalParameters nastavení musí přesně shodovat hello názvy v experimentech hello. Datová část požadavku ukázka hello můžete zobrazit na stránce nápovědy spuštění dávky hello pro vaši Azure ML koncový bod tooverify hello očekává mapování.

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a>Webová služba nevyžaduje žádný vstup
Azure ML dávky spuštění webové služby může být použité toorun žádný pracovní postup, například R nebo Python skripty, které ale nemusí vyžadovat všechny vstupy. Nebo pomocí čtečky modulu, který nevystavuje žádné GlobalParameters mohou být nakonfigurované hello experimentu. V takovém případě hello AzureMLBatchExecution aktivity by být nakonfigurovány takto:

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a>Webová služba nevyžaduje vstupu a výstupu
Hello Azure ML dávky spuštění webové služby, nemusí mít žádný výstup webové služby nakonfigurované. V tomto příkladu není žádná webová služba vstup nebo výstup, ani nejsou nakonfigurované žádné GlobalParameters. Stále je nakonfigurované na hello aktivity samotný výstup, ale není zadané jako webServiceOutput.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-hello-activity-runs-only-when-other-activities-have-succeeded"></a>Webové služby používá čtení a zápis a hello aktivity spustí jenom v případě, že ostatní aktivity proběhlo úspěšně.
Hello Azure ML web čtení a zápis modulů služby může být nakonfigurované toorun s nebo bez jakékoli GlobalParameters. Můžete však tooembed služby volá v kanál, který používá datovou sadu závislosti tooinvoke hello službu jenom v případě, že některé nadřazeného zpracování byla dokončena. Můžete ji spustit taky některých jiných akcí po dokončení spuštění dávek hello použití tohoto přístupu. V takovém případě lze vyjádřit pomocí aktivity vstupy a výstupy, bez pojmenování některý z nich jako webová služba vstupů nebo výstupů závislosti hello.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

Hello **takeaways** jsou:

* Pokud váš koncový bod experiment používá webServiceInput: to je reprezentována datovou sadu objektu blob a je součástí vstupech do aktivity hello a vlastnost webServiceInput hello. Vlastnost webServiceInput hello, jinak je vynechán.
* Pokud váš koncový bod experiment používá webServiceOutput(s): jejich jsou reprezentované pomocí datové sady objektu blob a jsou zahrnuty v hello výstupů aktivity a ve vlastnosti webServiceOutputs hello. výstupem aktivity Hello a webServiceOutputs jsou mapovány podle názvu hello každý výstupu v experimentu hello. Vlastnosti webServiceOutputs hello, jinak je vynechán.
* Pokud váš koncový bod experimentu zpřístupní globalParameter(s), jsou uvedené ve vlastnosti globalParameters aktivity hello jako páry klíčů, hodnota. Vlastnosti globalParameters hello, jinak je vynechán. Hello klíče jsou malá a velká písmena. [Azure Data Factory funkce](data-factory-functions-variables.md) je možné použít hodnoty hello.
* Další datové sady může být součástí vstupy a výstupy vlastnosti aktivity hello bez se na ně odkazovat v rámci typeProperties hello aktivity. Tyto datové sady řídí provádění pomocí závislosti řezu ale ignorují jinak hello AzureMLBatchExecution aktivity.


## <a name="updating-models-using-update-resource-activity"></a>Aktualizace modelů pomocí aktivita prostředku aktualizace
Po dokončení práce s retraining, aktualizujte hello vyhodnocování webové služby (zveřejněné jako webové služby prediktivní experiment) s nově trained model hello pomocí hello **aktivita prostředku aktualizace Azure ML**. V tématu [aktualizace modelů pomocí aktivita prostředku aktualizace](data-factory-azure-ml-update-resource-activity.md) článku.

### <a name="reader-and-writer-modules"></a>Čtečka a zapisovače moduly
Obvyklým scénářem pro používání parametry webové služby je hello používání Azure SQL čtení a zápis. modul čtečky Hello je použité tooload data do experimentu z datových služeb správy mimo Azure Machine Learning Studio. modul zapisovače Hello je toosave data z experimentů do služby pro data mimo Azure Machine Learning Studio.  

Podrobnosti o čtečky nebo zápis Azure Blob nebo Azure SQL najdete v tématu [čtečky](https://msdn.microsoft.com/library/azure/dn905997.aspx) a [zapisovače](https://msdn.microsoft.com/library/azure/dn905984.aspx) témata v knihovně MSDN. Příklad Hello v předchozí části hello používá hello objektů Blob v Azure čtení a zápis objektů Blob v Azure. Tato část popisuje používání Azure SQL čtení a zápis Azure SQL.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
**Otázka:** mám více souborů, které jsou generovány nástrojem Moje velkých datových kanálů. Můžete použít hello AzureMLBatchExecution aktivity toowork u všech souborů hello?

**Odpověď:** Ano. V tématu hello **pomocí čtečky modulu tooread dat z více souborů v Azure Blob** podrobnosti.

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML dávkové vyhodnocování aktivity
Pokud používáte hello **AzureMLBatchScoring** aktivity toointegrate pomocí Azure Machine Learning, doporučujeme používat nejnovější hello **AzureMLBatchExecution** aktivity.

Hello AzureMLBatchExecution aktivita byla zavedená v hello srpen 2015 verzi sady Azure SDK a prostředí Azure PowerShell.

Pokud chcete toocontinue pomocí hello AzureMLBatchScoring aktivity, pokračujte v načítání prostřednictvím této části.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Použití služby Azure Storage pro vstupní a výstupní Azure aktivity ML dávkového vyhodnocování

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a>Parametry webové služby
Přidat toospecify hodnoty pro parametry webové služby, **rámci typeProperties** části toohello **AzureMLBatchScoringActivty** část v kanálu hello JSON, jak ukazuje následující příklad hello:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
Můžete také použít [funkce objektu pro vytváření dat](data-factory-functions-variables.md) v předávání hodnot pro hello parametry webové služby, jak je znázorněno v hello následující ukázka:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> parametry webové služby Hello malá a velká písmena, zajistěte proto, že hello názvy, které zadáte v aktivitě hello JSON odpovídají hello ty, které jsou vystavené hello webové služby.
>
>

## <a name="see-also"></a>Viz také
* [Azure příspěvku na blogu: Začínáme s Azure Data Factory a Azure Machine Learning](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
