---
title: "Kopírování dat z Amazon jednoduché úložiště služby pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak zkopírovat data z Amazon jednoduché úložiště služby (S3) do úložiště podporované jímku dat pomocí Azure Data Factory."
services: data-factory
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 6df7d74d572a59c83105905fbe0a9e218aadc28f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Kopírování dat z Amazon jednoduché úložiště služby pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Verze 2 – Preview](connector-amazon-simple-storage-service.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat do a z Azure Blob Storage. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [Amazon S3 connnector v V1](v1/data-factory-amazon-simple-storage-service-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete zkopírovat data z jakékoli úložiště podporované zdroje dat do Azure Data Lake Store nebo kopírování dat z Azure Data Lake Store do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor Amazon S3 podporuje kopírování souborů jako-je nebo analýza souborů pomocí [podporované formáty souborů a komprese kodeky](supported-file-formats-and-compression-codecs.md).

## <a name="required-permissions"></a>Požadovaná oprávnění

Pokud chcete zkopírovat data z Amazonu S3, zkontrolujte, zda že máte následující oprávnění:

- `s3:GetObject`a `s3:GetObjectVersion` pro Amazon S3 objekt operace.
- `s3:ListBucket`pro operace sady Amazon S3. Pokud použijete Průvodce kopírováním objekt pro vytváření dat `s3:ListAllMyBuckets` je také nutný.

Podrobnosti o úplný seznam Amazon S3 oprávnění najdete v tématu [zadání oprávnění v zásadách](http://docs.aws.amazon.com/amazons3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory k Amazon S3.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro Amazon S3 propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavená na **AmazonS3**. | Ano |
| accessKeyId | ID tajný přístupový klíč. |Ano |
| secretAccessKey | Tajný přístupový klíč sám sebe. Toto pole můžete označte jako SecureString. |Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je vaše úložiště dat se nachází v privátní síti), můžete použít modul Runtime integrace Azure nebo Self-hosted integrace Runtime. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

>[!NOTE]
>Tento konektor vyžaduje přístupové klíče pro účet IAM ke zkopírování dat z Amazon S3. [Dočasné pověření zabezpečení](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) není podporován.
>

Zde naleznete příklad:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                    "type": "SecureString",
                    "value": "<secret access key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností nepodporuje Amazon S3 datovou sadu.

Chcete-li kopírovat data z Amazonu S3, nastavte vlastnost typu datové sady, která **AmazonS3Object**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **AmazonS3Object** |Ano |
| bucketName | Název sady S3. |Ano |
| key | Klíč objektu S3. Platí, pouze pokud není zadán předponu. |Ne |
| Předpona | Předpona pro klíč objektu S3. Jsou vybrané objekty, jejichž klíče začít s touto předponou. Platí, pouze pokud není zadán klíč. |Ne |
| verze | Verze objektu S3, pokud je povolena Správa verzí S3. |Ne |
| Formát | Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.<br/><br/>Pokud chcete analyzovat nebo vygenerování soubory s konkrétním formátu, jsou podporovány následující typy souboru formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot. Další informace najdete v tématu [textovém formátu](supported-file-formats-and-compression-codecs.md#text-format), [formátu Json](supported-file-formats-and-compression-codecs.md#json-format), [Avro formát](supported-file-formats-and-compression-codecs.md#avro-format), [Orc formátu](supported-file-formats-and-compression-codecs.md#orc-format), a [Parquet formát](supported-file-formats-and-compression-codecs.md#parquet-format) oddíly. |Ne (pouze pro scénář binární kopie) |
| Komprese | Zadejte typ a úroveň komprese pro data. Další informace najdete v tématu [podporované formáty souborů a komprese kodeky](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.<br/>Jsou podporované úrovně: **Optimal** a **nejrychlejší**. |Ne |

> [!NOTE]
> **bucketName + klíč** Určuje umístění objektu S3, kde sady je kořenový kontejner pro objekty S3 a klíč je úplná cesta k objektu S3.

**Příklad: pomocí předpony**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Příklad: pomocí klíče a verze (volitelné)**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Vlastnosti aktivity kopírování

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností, které podporuje Azure Data Lake zdroj a jímka.

### <a name="amazon-s3-as-source"></a>Amazon S3 jako zdroj

Pokud chcete zkopírovat data z Amazonu S3, nastavit typ zdroje v aktivitě kopírování do **FileSystemSource** (která zahrnuje Amazon S3). Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **FileSystemSource** |Ano |
| Rekurzivní | Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky. Poznámka: když rekurzivní nastavena na hodnotu true a jímka je na základě souborů úložiště, prázdné složky nebo dílčí-folder nebudou zkopírovat nebo vytvořit v jímky.<br/>Povolené hodnoty jsou: **true** (výchozí), **false** | Ne |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).