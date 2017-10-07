---
title: "aaaMove data z databáze MySQL pomocí Azure Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z databáze MySQL databáze pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jingwang
ms.openlocfilehash: 3ffe969e42ce1a54b265c4739df43fdc594ea891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a>Přesun dat pomocí Azure Data Factory z databáze MySQL
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze MySQL na místě. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Z úložiště úložiště tooany podporované jímku dat místní MySQL dat může kopírovat data. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat tooother uložit data z databáze MySQL data, ale ne pro přesun dat z jiného úložiště dat úložiště tooan MySQL data. 

## <a name="prerequisites"></a>Požadavky
Služba data Factory podporuje připojování zdroje MySQL tooon místní pomocí hello Brána pro správu dat. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.

Vyžaduje se brána, i když je databáze MySQL hello hostované ve virtuálním počítači Azure IaaS (VM). Hello brány můžete nainstalovat na hello stejného virtuálního počítače jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit toohello databáze.

> [!NOTE]
> V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.

## <a name="supported-versions-and-installation"></a>Podporované verze a instalaci
Brána pro správu dat tooconnect toohello databáze MySQL, musíte tooinstall hello [MySQL Connector/Net pro Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (verze 6.6.5 nebo vyšší) na stejné hello systému jako brána pro správu dat hello. MySQL verze 5.1 a vyšší je podporovaná.

> [!TIP]
> Chyba při dosažení "Ověřování se nezdařilo, protože je uzavřený vzdálené strany hello hello přenosu datového proudu.", zvažte tooupgrade hello MySQL Connector/Net toohigher verze.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API. 

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello. 
- Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat místní MySQL, naleznete v tématu [JSON příklad: kopírování dat z databáze MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa MySQL úložiště dat:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooMySQL propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **OnPremisesMySql** |Ano |
| server |Název serveru MySQL hello. |Ano |
| Databáze |Název databáze MySQL hello. |Ano |
| Schéma |Název schématu hello v databázi hello. |Ne |
| authenticationType. |Typ ověřování používá databázi MySQL toohello tooconnect. Možné hodnoty jsou: `Basic`. |Ano |
| uživatelské jméno |Zadejte uživatelské jméno tooconnect toohello MySQL databázi. |Ano |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali. |Ano |
| gatewayName |Název hello brány, kterou hello služba Data Factory by měl použít toohello tooconnect, místní databázi MySQL. |Ano |

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **RelationalTable** (což zahrnuje datová sada MySQL) má následující vlastnosti hello

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v hello instance databáze MySQL, která je propojená služba odkazuje. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.

Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje MySQL), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Příklad: vybrat * z MyTable. |Ne (Pokud **tableName** z **datovou sadu** je zadána) |


## <a name="json-example-copy-data-from-mysql-tooazure-blob"></a>Příklad JSON: kopírování dat z databáze MySQL tooAzure objektů Blob
Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazuje, jak toocopy data z databáze MySQL místní databázi tooan Azure Blob Storage. Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

> [!IMPORTANT]
> Tato ukázka obsahuje fragmenty kódu JSON. Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.

Ukázka Hello má hello následující entity objektu pro vytváření dat:

1. Propojené služby typu [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databáze MySQL každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Jako první krok nastavte Brána pro správu dat hello. Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

**MySQL propojené služby:**

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

**Propojená služba Azure Storage:**

```JSON
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

**MySQL vstupní datové sady:**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v MySQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.

Nastavení "externí": "PRAVDA" informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
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
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kanál s aktivitou kopírování:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.

```JSON
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a>Mapování typu pro databázi MySQL
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesunu dat tooMySQL, hello následující mapování se používají z databáze MySQL typy too.NET typů.

| Typ databáze MySQL | Typ rozhraní .NET framework |
| --- | --- |
| bigint bez znaménka |Decimal |
| bigint |Int64 |
| Bit |Decimal |
| Objekt BLOB |Byte] |
| BOOL |Logická hodnota |
| Char |Řetězec |
| Datum |Data a času |
| Data a času |Data a času |
| Decimal |Decimal |
| Dvojitá přesnost |Double |
| Double |Double |
| výčet |Řetězec |
| Plovoucí desetinná čárka |Jeden |
| int bez znaménka |Int64 |
| celá čísla |Int32 |
| celé číslo bez znaménka |Int64 |
| celé číslo |Int32 |
| dlouhé varbinary |Byte] |
| dlouhé varchar |Řetězec |
| longblob |Byte] |
| LONGTEXT |Řetězec |
| mediumblob |Byte] |
| mediumint bez znaménka |Int64 |
| mediumint |Int32 |
| mediumtext |Řetězec |
| číselné |Decimal |
| skutečné |Double |
| nastavení |Řetězec |
| smallint bez znaménka |Int32 |
| smallint |Int16 |
| Text |Řetězec |
| time |Časový interval |
| časové razítko |Data a času |
| tinyblob |Byte] |
| tinyint bez znaménka |Int16 |
| tinyint |Int16 |
| tinytext |Řetězec |
| varchar |Řetězec |
| Rok |celá čísla |

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
