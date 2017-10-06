---
title: "aaaMove data ze služby Amazon jednoduché úložiště pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data ze služby úložiště jednoduché Amazon (S3) pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8a8cd2845fd1de74413bd0372f3aabfb4817549b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Přesun dat ze služby Amazon jednoduché úložiště pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivita kopírování v Azure Data Factory toomove data ze služby úložiště jednoduché Amazon (S3). Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Může kopírovat data z úložiště dat podřízený Amazon S3 tooany podporována. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z úložiště dat tooother Amazon S3, ale není přesouvání dat od ostatních dat ukládá tooAmazon S3.

## <a name="required-permissions"></a>Požadovaná oprávnění
toocopy data z Amazonu S3, zajistěte, aby vám byla udělena hello následující oprávnění:

* `s3:GetObject`a `s3:GetObjectVersion` pro Amazon S3 objekt operace.
* `s3:ListBucket`pro operace sady Amazon S3. Pokud používáte hello Průvodce kopírováním objekt pro vytváření dat, `s3:ListAllMyBuckets` je také nutný.

Podrobnosti o hello úplný seznam Amazon S3 oprávnění najdete v tématu [zadání oprávnění v zásadách](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje Amazon S3 s použitím rozhraní API nebo jiných nástrojů.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. Rychlé návod najdete v tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. Podrobné pokyny toocreate kanál s aktivitou kopírování, najdete v části hello [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Pokud používáte rozhraní API (s výjimkou .NET API) nebo nástroje, definujete tyto entity služby Data Factory pomocí formátu JSON hello. Ukázka s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat Amazon S3, najdete v části hello [JSON příklad: kopírování dat z Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) tohoto článku.

> [!NOTE]
> Podrobnosti o podporovaných formátech souborů a komprese pro aktivitu kopírování najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAmazon S3.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Propojená služba odkazuje data store tooa objekt pro vytváření dat. Vytvoření propojené služby typu **AwsAccessKey** toolink Amazon S3 data ukládat tooyour data factory. Hello následující tabulka obsahuje popis JSON elementy konkrétní tooAmazon S3 (AwsAccessKey) propojené služby.

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| accessKeyID |ID hello tajný přístupový klíč. |Řetězec |Ano |
| secretAccessKey |Hello tajný přístupový klíč, sám sebe. |Šifrované tajné řetězec |Ano |

Zde naleznete příklad:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
toospecify toorepresent datovou sadu vstupní data v úložišti objektů Blob v Azure, nastavte vlastnost typu hello sady dat hello příliš**AmazonS3**. Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Amazon S3 propojené služby. Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md). 

Oddíly jako je například struktura, dostupnost a zásady jsou podobné pro všechny typy datové sady (například databáze SQL, objektů blob v Azure a tabulky Azure). Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. Hello **rámci typeProperties** části datové sady typu **AmazonS3** (což zahrnuje datová sada hello Amazon S3) má hello následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| bucketName |Název sady Hello S3. |Řetězec |Ano |
| key |klíč objektu Hello S3. |Řetězec |Ne |
| Předpona |Předpona pro klíč objektu S3 hello. Jsou vybrané objekty, jejichž klíče začít s touto předponou. Platí pouze v případě, klíč je prázdný. |Řetězec |Ne |
| Verze |verze Hello hello S3 objektu, pokud je povolena Správa verzí S3. |Řetězec |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu hello [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format), [formát Avro](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formátu ](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete, aby soubory toocopy jako-mezi souborové úložiště (binární kopie), přeskočit hello formátu část v obou definice vstupní a výstupní datové sady. |Ne | |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Hello podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. jsou Hello podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne | |


> [!NOTE]
> **bucketName + klíč** Určuje umístění hello hello S3 objektu, kde je sady hello Kořenový kontejner pro objekty S3 a klíč je objekt toohello S3 hello úplnou cestu.

### <a name="sample-dataset-with-prefix"></a>Ukázkovou datovou sadu s předponou

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
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
### <a name="sample-dataset-with-version"></a>Ukázkovou datovou sadu (verze)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
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

### <a name="dynamic-paths-for-s3"></a>Dynamické cesty pro S3
Hello předchozí ukázce se používá pevné hodnoty pro hello **klíč** a **bucketName** vlastnosti v datové sadě hello Amazon S3.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

Můžete mít vypočítat tyto vlastnosti dynamicky za běhu, pomocí systémové proměnné, jako je například SliceStart služby Data Factory.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Můžete provést stejný hello pro hello **předponu** vlastnost datové sadě služby Amazon S3. Seznam podporovaných funkce a proměnné, naleznete v části [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md). Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit. Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování hello vlastnosti lišit v závislosti na typech hello zdrojů a jímky. Pokud je zdroj v aktivitě kopírování hello typu **FileSystemSource** (která zahrnuje Amazon S3), hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda seznam toorecursively S3 objekty v adresáři hello. |hodnotu true nebo false |Ne |

## <a name="json-example-copy-data-from-amazon-s3-tooazure-blob-storage"></a>Příklad JSON: kopírování dat z Amazon S3 tooAzure úložiště objektů Blob
Tento příklad ukazuje, jak toocopy data z Amazon S3 tooan úložiště objektů Blob v Azure. Ale data se dají zkopírovat přímo příliš[žádné hello jímky, které jsou podporovány](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování hello v datové továrně.

Ukázka Hello poskytuje JSON definice hello následující entity služby Data Factory. Můžete použít tyto definice toocreate kanálu toocopy dat z úložiště tooBlob Amazon S3, pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).   

* Propojené služby typu [AwsAccessKey](#linked-service-properties).
* Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Vstup [datovou sadu](data-factory-create-datasets.md) typu [AmazonS3](#dataset-properties).
* Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z Amazon S3 tooan objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

### <a name="amazon-s3-linked-service"></a>Amazon S3 propojené služby

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="amazon-s3-input-dataset"></a>Vstupní datové sady Amazon S3

Nastavení **"externí": true** informuje o tom služba Data Factory hello tuto datovou sadu hello je externí toohello data factory. Nastavte tuto vlastnost tootrue vstupní datovou sadu, která není vyprodukované aktivitu v kanálu hello.

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
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


### <a name="azure-blob-output-dataset"></a>Výstupní datová sada Azure Blob

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá hello rok, měsíc, den a čas části hello počáteční čas.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>Aktivita kopírování v kanálu s na Amazon S3 zdroj a jímka objektů blob

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady, a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource**, a **podřízený** je typ nastaven příliš**BlobSink**.

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> toomap sloupce z toocolumns datovou sadu zdroj z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).


## <a name="next-steps"></a>Další kroky
V tématu hello následující články:

* toolearn o klíči faktory, že dopad výkonu (aktivita kopírování) přesun dat v datové továrně a různé způsoby toooptimize, najdete v části hello [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md).

* Podrobné pokyny pro vytvoření kanálu s aktivitou kopírování najdete v tématu hello [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
