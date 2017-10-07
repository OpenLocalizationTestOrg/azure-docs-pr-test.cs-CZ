---
title: "aaaMove data z PostgreSQL pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data z databáze PostgreSQL pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: ea384f4e06f7d7bedae2949e4ea727c8f8806614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Přesun dat z PostgreSQL pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze PostgreSQL místně. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní PostgreSQL data. Seznam úložišť dat jako jímky nepodporuje hello aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Objekt pro vytváření dat aktuálně podporuje přesun dat z PostgreSQL databáze tooother úložiště dat, ale není pro přesun dat z jiných dat ukládá tooan databázi PostgreSQL. 

## <a name="prerequisites"></a>Požadavky

Služba data Factory podporuje připojování zdroje PostgreSQL tooon místní pomocí hello Brána pro správu dat. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.

Vyžaduje se brána, i když je databáze PostgreSQL hello hostována v virtuálního počítače Azure IaaS. Brány můžete nainstalovat na stejný virtuální počítač IaaS jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit databázi toohello hello.

> [!NOTE]
> V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.

## <a name="supported-versions-and-installation"></a>Podporované verze a instalaci
Brána pro správu dat tooconnect toohello databázi PostgreSQL, nainstalujte hello [Ngpsql zprostředkovatele dat pro PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 nebo výše na hello stejné systému jako brána pro správu dat hello. PostgreSQL verze 7.4 a vyšší je podporovaná.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní PostgreSQL data pomocí různých nástrojů nebo rozhraní API. 

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello. 
- Také můžete použít následující nástroje toocreate kanálu hello: 
    - portál Azure
    - Visual Studio
    - Azure PowerShell
    - Šablona Azure Resource Manageru
    - .NET API
    - REST API

     V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z místního úložiště dat PostgreSQL, naleznete v tématu [JSON příklad: kopírování dat z PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa PostgreSQL úložiště dat:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooPostgreSQL propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **OnPremisesPostgreSql** |Ano |
| server |Název serveru PostgreSQL hello. |Ano |
| Databáze |Název databáze PostgreSQL hello. |Ano |
| Schéma |Název schématu hello v databázi hello. název schématu Hello rozlišuje velká a malá písmena. |Ne |
| authenticationType. |Typ ověřování používá databázi PostgreSQL toohello tooconnect. Možné hodnoty jsou: anonymní, základní a systému Windows. |Ano |
| uživatelské jméno |Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi PostgreSQL. |Ano |

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.

část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **RelationalTable** (což zahrnuje datová sada PostgreSQL) má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v hello instance databáze PostgreSQL, která je propojená služba odkazuje. Hello tableName rozlišuje velká a malá písmena. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pokud je zdroj typu **RelationalSource** (která zahrnuje PostgreSQL), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: "dotaz": "vybrat * z \"MySchema\".\" MyTable\"". |Ne (Pokud **tableName** z **datovou sadu** je zadána) |

> [!NOTE]
> Schéma a tabulku názvy rozlišují malá a velká písmena. Uzavřete je do `""` (dvojité uvozovky) v dotazu hello.  

**Příklad:**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-tooazure-blob"></a>Příklad JSON: kopírování dat z PostgreSQL tooAzure objektů Blob
Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak toocopy data z PostgreSQL databáze tooAzure úložiště objektů Blob. Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.   

> [!IMPORTANT]
> Tato ukázka obsahuje fragmenty kódu JSON. Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.

Ukázka Hello má hello následující entity objektu pro vytváření dat:

1. Propojené služby typu [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databáze PostgreSQL každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Jako první krok nastavte Brána pro správu dat hello. Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

**PostgreSQL propojené služby:**

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
**Propojená služba Azure Blob storage:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
    }
    }
}
```
**PostgreSQL vstupní datové sady:**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v PostgreSQL a obsahuje sloupec s názvem "časové razítko" pro data časové řady.

Nastavení `"external": true` informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
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

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data z hello public.usstates tabulky v databázi PostgreSQL hello.

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
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
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a>Mapování typu pro PostgreSQL
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesunu dat tooPostgreSQL, hello následující mapování se používají z typu too.NET typu PostgreSQL.

| Typ databáze PostgreSQL | PostgresSQL aliasy | Typ rozhraní .NET framework |
| --- | --- | --- |
| abstime | |Data a času | &nbsp;
| bigint |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bit [(ne)] | |Byte [] řetězec | &nbsp;
| bit různých [(ne)] |varbit |Byte [] řetězec |
| Logická hodnota |BOOL |Logická hodnota |
| Pole | |Byte [] řetězec |&nbsp;
| bytea | |Byte [] řetězec |&nbsp;
| znak [(ne)] |char [(ne)] |Řetězec |
| znak různých [(ne)] |varchar [(ne)] |Řetězec |
| CID | |Řetězec |&nbsp;
| CIDR | |Řetězec |&nbsp;
| kruhu. | |Byte [] řetězec |&nbsp;
| Datum | |Data a času |&nbsp;
| DateRange | |Řetězec |&nbsp;
| Dvojitá přesnost |FLOAT8 |Double |
| inet | |Byte [] řetězec |&nbsp;
| intarry | |Řetězec |&nbsp;
| int4range | |Řetězec |&nbsp;
| int8range | |Řetězec |&nbsp;
| celé číslo |int, int4 |Int32 |
| Interval [pole] [(p)] | |Časový interval |&nbsp;
| JSON | |Řetězec |&nbsp;
| jsonb | |Byte] |&nbsp;
| řádek | |Byte [] řetězec |&nbsp;
| lseg | |Byte [] řetězec |&nbsp;
| macaddr | |Byte [] řetězec |&nbsp;
| peníze | |Decimal |&nbsp;
| číselný [(p, s)] |Decimal [(p, s)] |Decimal |
| numrange | |Řetězec |&nbsp;
| OID | |Int32 |&nbsp;
| Cesta | |Byte [] řetězec |&nbsp;
| pg_lsn | |Int64 |&nbsp;
| bod | |Byte [] řetězec |&nbsp;
| mnohoúhelníku | |Byte [] řetězec |&nbsp;
| skutečné |FLOAT4 |Jeden |
| smallint |int2 |Int16 |
| smallserial |serial2 |Int16 |
| Sériového portu |serial4 |Int32 |
| Text | |Řetězec |&nbsp;

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
