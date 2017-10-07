---
title: aaaMove data do/z Azure Table | Microsoft Docs
description: "Zjistěte, jak toomove data do nebo z úložiště tabulek Azure pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 3dc3da6d88854674a9108b600534bc5d07575f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-table-using-azure-data-factory"></a>Přesun dat tooand z Azure Table pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove dat z úložiště tabulek Azure. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello. 

Data můžete zkopírovat z jakéhokoli podporované zdroje dat ukládání tooAzure Table Storage nebo z Azure Table Storage tooany podporované jímku dat úložiště. Seznam úložišť dat jako zdroje nebo jímky podporované aktivitou kopírování hello najdete v tématu hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. 

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Table Storage pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo ze Azure Table Storage, najdete v části [JSON příklady](#json-examples) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure Table Storage: 

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Existují dva typy propojené služby můžete použít toolink služby Azure blob storage tooan Azure data factory. Jsou: **azurestorage** propojená služba a **AzureStorageSas** propojené služby. Hello propojené služby Azure Storage poskytuje objekt pro vytváření dat hello s globálním přístupem toohello Azure Storage. Zatímco hello Azure úložiště SAS (sdíleného přístupového podpisu) propojená služba poskytuje objekt pro vytváření dat hello s přístup omezený nebo časově vázaných toohello Azure Storage. Nejsou žádné další rozdíly mezi tyto dvě propojené služby. Zvolte hello propojené služby, který vyhovuje vašim potřebám. Hello následující oddíly poskytují další podrobnosti o tyto dvě propojené služby.

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. Hello **rámci typeProperties** části pro datovou sadu hello typu **AzureTable** má následující vlastnosti hello.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello hello instance Azure tabulku databáze, kterou propojená služba odkazuje. |Ano. Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový. Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový. |

### <a name="schema-by-data-factory"></a>Schéma službou Data Factory
Pro data bez schémat úložiště, jako je například Azure Table odvodí hello služba Data Factory hello schéma v jednom z následujících způsobů hello:

1. Pokud zadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, hello služba Data Factory ctí tato struktura jako hello schématu. V takovém případě Pokud řádek neobsahuje hodnotu pro sloupec, je pro něj zadat hodnotu null.
2. Pokud nezadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, Data Factory odvodí hello schématu pomocí hello první řádek v datech hello. V takovém případě pokud první řádek hello neobsahuje úplnou schématu hello, některé sloupce naplánované v hello výsledek operace kopírování.

Proto pro zdroje dat bez schémat, hello osvědčeným postupem je toospecify hello strukturu dat pomocí hello **struktura** vlastnost.

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní datové sady a zásad jsou dostupné pro všechny typy aktivit.

Vlastnosti dostupné v rámci typeProperties části hello hello aktivit na hello druhé straně lišit každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

**AzureTableSource** podporuje hello následující vlastnosti v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| azureTableSourceQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu tabulky Azure. Příklady v další části hello. |Ne. Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový. Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový. |
| azureTableSourceIgnoreTableNotFound |Označuje, zda swallow hello výjimky tabulky neexistuje. |HODNOTA TRUE<br/>FALSE |Ne |

### <a name="azuretablesourcequery-examples"></a>Příklady azureTableSourceQuery
Pokud sloupec tabulky Azure je typu řetězec:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Pokud sloupec tabulky Azure je typu datum a čas:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink** podporuje hello následující vlastnosti v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Výchozí hodnotu klíče oddílu, mohou být využívána hello jímky. |Hodnotu řetězce. |Ne |
| azureTablePartitionKeyName |Zadejte název hello sloupce, jejichž hodnoty se používají jako klíče oddílů. Pokud není zadaný, použije se jako klíč oddílu hello AzureTableDefaultPartitionKeyValue. |Název sloupce. |Ne |
| azureTableRowKeyName |Zadejte název hello sloupce, jejichž hodnoty sloupce jsou použity jako klíč řádku. Pokud není zadaný, použijte identifikátor GUID pro každý řádek. |Název sloupce. |Ne |
| azureTableInsertType |Hello režimu tooinsert data do tabulky Azure.<br/><br/>Tato vlastnost určuje, jestli mají existující řádky v tabulce výstup hello s odpovídajícím klíče oddílu a řádku jejich hodnoty nahradit nebo sloučit. <br/><br/>toolearn o tom, jak tato nastavení (sloučení a nahraďte) fungují, najdete v části [vložení nebo sloučení Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) a [vložení nebo nahrazení Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) témata. <br/><br> Toto nastavení se vztahuje na úrovni řádku hello, není hello na úrovni tabulky, a ani možnost odstraní řádky v tabulce výstup hello, které nejsou k dispozici ve vstupu hello. |Merge (výchozí)<br/>Nahradit |Ne |
| writeBatchSize |Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure. |Celé číslo (počet řádků) |Ne (výchozí: 10000) |
| writeBatchTimeout |Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure |Časový interval<br/><br/>Příklad: "00:20:00" (20 minut) |Ne (výchozí hodnota časového limitu pro klienta toostorage výchozí hodnotu 90 sekundu) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Mapovat zdrojový sloupec tooa cílový sloupec pomocí hello překladač JSON vlastnost jako hello azureTablePartitionKeyName, abyste mohli používat hello cílový sloupec.

V následujícím příkladu hello, zdrojový sloupec DivisionID je namapované toohello cílový sloupec: DivisionID.  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
Hello DivisionID je zadán jako klíč oddílu hello.

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a>Příklady JSON
Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak toocopy tooand dat z úložiště tabulek Azure a Azure Blob databáze. Nicméně je možné zkopírovat data **přímo** z jakéhokoli z tooany zdroje hello z jímky hello podporována. Další informace najdete v tématu hello části "podporované úložiště dat a formáty" v [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md).

## <a name="example-copy-data-from-azure-table-tooazure-blob"></a>Příklad: Kopírování dat z Azure Table tooAzure objektů Blob
Následující ukázka ukazuje Hello:

1. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties) (používá se pro objekt blob & tabulky).
2. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureTable](#dataset-properties).
3. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [AzureTableSource](#activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data patřící toohello výchozí oddíl v objektu blob Azure Table tooa každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Propojená služba úložiště Azure:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**. Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS). V tématu [propojené služby](#linked-service-properties) podrobnosti.  

**Azure Table vstupní datové sady:**

Ukázka Hello předpokládá, že jste vytvořili tabulku "MyTable" v Azure Table.

Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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

**Azure Blob výstupní datovou sadu:**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Aktivita kopírování v kanálu s AzureTableSource a BlobSink:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**AzureTableSource** a **podřízený** je typ nastaven příliš**BlobSink**. Zadaný dotaz SQL Hello **AzureTableSourceQuery** vlastnost vybere hello dat z oddílu výchozí hello toocopy každou hodinu.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                      {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                      {
                            "name": "AzureBlobOutput"
                      }
                ],
                "typeProperties": {
                      "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                },
                "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },                
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
            }
         ]    
    }
}
```

## <a name="example-copy-data-from-azure-blob-tooazure-table"></a>Příklad: Kopírování dat z Azure Blob tooAzure tabulky
Následující ukázka ukazuje Hello:

1. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties) (používá se pro objekt blob & tabulky)
2. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
3. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureTable](#dataset-properties).
4. Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [AzureTableSink](#copy-activity-properties).

Ukázka Hello zkopíruje data časové řady ze objektů blob v Azure tooan tabulky Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Propojená služba Azure storage (pro Azure Table & objektů Blob):**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**. Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS). V tématu [propojené služby](#linked-service-properties) podrobnosti.

**Azure vstupní datovou sadu objektu Blob:**

Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas. "externí": "PRAVDA" nastavení informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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

**Tabulky Azure výstupní datovou sadu:**

Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v tabulce Azure. Vytvoření Azure tabulky s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello. Přidávání řádků tabulky toohello každou hodinu.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Aktivita kopírování v kanálu s BlobSource a AzureTableSink:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**AzureTableSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
          }
        },
        "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },                        
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
## <a name="type-mapping-for-azure-table"></a>Typ mapování pro tabulku Azure
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující dvoustupňový přístup.

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesouvání dat příliš & z tabulky Azure, hello následující [mapování definovaná službou Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) se používají z typu too.NET typy OData tabulky Azure a naopak.

| Typ dat OData | Typ formátu .NET | Podrobnosti |
| --- | --- | --- |
| Edm.Binary |Byte] |Pole bajtů až too64 KB. |
| Edm.Boolean |BOOL |Logická hodnota. |
| Edm.DateTime |Data a času |Hodnota 64-bit, vyjádřené jako koordinovaný světový čas (UTC). Hello podporované rozsah začíná na 12:00 hodin, 1, 1601. ledna data a času (C.E.), UTC. rozsah Hello končí u 31. prosince 9999. |
| Edm.Double |Double |Hodnota 64-bit plovoucí bodu. |
| Edm.Guid |Identifikátor GUID |Globálně jedinečný identifikátor 128-bit. |
| Edm.Int32 |Int32 |32bitové celé číslo. |
| Edm.Int64 |Int64 |64bitové celé číslo. |
| Edm.String |Řetězec |Hodnota kódování UTF-16. Řetězcové hodnoty může mít až too64 KB. |

### <a name="type-conversion-sample"></a>Ukázka převod typu
Následující ukázka Hello je pro kopírování dat z Azure Blob tooAzure tabulky s převody typů.

Předpokládejme, že datovou sadu objektu Blob hello je ve formátu CSV a obsahuje tři sloupce. Jeden z nich je sloupec Datum a čas ve formátu data a času vlastní pomocí zkrácené francouzštině názvy den v týdnu hello.

Definujte datovou sadu objektu Blob zdroj hello takto společně s definic typů pro sloupce hello.

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
         "structure":
          [
                { "name": "userid", "type": "Int64"},
                { "name": "name", "type": "String"},
                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
Zadané mapování typu hello z typu too.NET typu OData tabulky Azure, by definovat hello tabulky v tabulce Azure s hello následující schéma.

**Schéma tabulky Azure:**

| Název sloupce | Typ |
| --- | --- |
| ID uživatele |Edm.Int64 |
| jméno |Edm.String |
| lastlogindate |Edm.DateTime |

V dalším kroku definujte datová sada Azure Table hello následujícím způsobem. Vzhledem k tomu, že informace o typu hello se už vyskytuje v hello základní úložiště dat nemusí toospecify "struktura" oddíl s informací o typu hello.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

V takovém případě Data Factory automaticky převody typu včetně hello pole data a času ve formátu data a času vlastní hello při přesouvání dat z objektu Blob tooAzure tabulce s využitím jazykové verze hello "fr-fr".

> [!NOTE]
> toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Výkon a ladění
výkon této dopad přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize faktory toolearn o klíči, najdete v části [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md).
