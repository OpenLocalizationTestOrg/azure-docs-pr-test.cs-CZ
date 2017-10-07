---
title: "aaaMove data z DB2 pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak toomove data z DB2 místní databázi pomocí aktivity kopírování objektu pro vytváření dat Azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 696ac059be644cb3901c37d2fc746e0682c65a1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Přesun dat z DB2 pomocí Azure Data Factory kopie aktivity
Tento článek popisuje, jak můžete použít aktivitu kopírování v Azure Data Factory toocopy dat z úložiště dat tooa databáze DB2 místně. Můžete zkopírovat tooany úložiště dat, která je uvedena jako podporovaných jímku v hello [aktivity přesunu dat pro vytváření dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) článku. Toto téma je založený na článek hello Data Factory, který zobrazí přehled o přesun dat pomocí aktivity kopírování a jsou uvedeny kombinace úložiště dat hello podporována. 

Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z databáze tooa DB2 [úložiště dat podporovaných podřízený](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Přesun dat z jiných dat ukládá tooa DB2 databáze není podporována.

## <a name="prerequisites"></a>Požadavky
Objekt pro vytváření dat podporuje připojování tooan místní databázi DB2 pomocí hello [Brána pro správu dat](data-factory-data-management-gateway.md). Podrobné pokyny tooset hello brány dat kanálu toomove dat naleznete v tématu hello [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku.

Vyžaduje se brána, i pokud hello DB2 je hostitelem virtuálního počítače Azure IaaS. Hello brány můžete nainstalovat na hello stejný virtuální počítač IaaS jako úložiště dat hello. Pokud brána hello se můžete připojit toohello databáze, můžete hello bránu nainstalovat na jiný virtuální počítač.

Brána pro správu dat Hello poskytuje integrované ovladače DB2, není nutné toomanually nainstalujte dat toocopy ovladače z DB2.

> [!NOTE]
> Tipy pro řešení problémů připojení a problémy brány najdete v tématu hello [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) článku.


## <a name="supported-versions"></a>Podporované verze
konektor Hello DB2 objekt pro vytváření dat podporuje hello IBM DB2 platformy a verze správce přístupu k SQL distribuované relační databáze architektura (DRDA) verze 9, 10 a 11:

* IBM DB2 pro z verze 11.1 operačního systému
* IBM DB2 pro z verze 10.1 operačního systému
* IBM DB2 verze i (AS400) 7.2
* IBM DB2 i (AS400) verze 7.1
* IBM DB2 pro Windows (LUW) verze 11, Linux a UNIX
* IBM DB2 pro LUW verze 10.5
* IBM DB2 pro LUW verze 10.1

> [!TIP]
> Pokud se zobrazí chybová zpráva hello "hello balíčku odpovídající tooan SQL příkaz žádost o spuštění nebyla nalezena. SQLSTATE = 51002 = SQLCODE-805, "hello důvod je potřebný balíček není vytvořena pro hello normální uživatele na hello operačního systému. tooresolve-li tento problém, postupujte podle těchto pokynů pro váš typ serveru DB2:
> - DB2 pro i (AS400): může uživatel power vytvoření kolekce hello hello normální uživatele před spuštěním aktivity kopírování. toocreate hello shromažďování, použití hello příkaz:`create collection <username>`
> - DB2 pro z/OS nebo LUW: použití účtu s vysokou úrovní oprávnění – power users nebo správce, který má balíček autority a vazby, BINDADD, UDĚLTE oprávnění spouštět tooPUBLIC – toorun hello jednou kopírování. Hello potřebný balíček je automaticky vytvoří během kopírování hello. Potom můžete přepnout zpět toohello normální pro vaše další kopie spustí.

## <a name="getting-started"></a>Začínáme
Můžete vytvoření kanálu s kopie aktivity toomove dat z úložiště dat DB2 místně pomocí různých nástrojů a rozhraní API: 

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello Průvodce kopírováním objekt pro vytváření dat Azure. Rychlé návod k vytvoření kanálu pomocí Průvodce kopírováním hello najdete v tématu hello [kurz: vytvoření kanálu pomocí Průvodce kopírováním hello](data-factory-copy-data-wizard-tutorial.md). 
- Můžete použít také nástroje toocreate kanál, včetně hello portálu Azure, Visual Studio, prostředí Azure PowerShell, Azure Resource Manager šablony, hello .NET API a hello REST API. Podrobné pokyny toocreate kanál s aktivitou kopírování, najdete v části hello [aktivity kopírování kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření propojené služby toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření datových sad toorepresent vstupní a výstupní data pro operace kopírování hello. 
3. Vytvoření kanálu s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Při použití hello Průvodce kopírováním, definice JSON pro hello Data Factory propojené služby, můžete vytvořit automaticky datových sad a kanálu entity. Při použití nástroje nebo rozhraní API (s výjimkou hello .NET API), můžete definovat hello entit služby Data Factory pomocí formátu JSON hello. Hello [JSON příklad: kopírování dat z DB2 tooAzure úložiště objektů Blob](#json-example-copy-data-from-db2-to-azure-blob) ukazuje hello JSON definice pro hello entit služby Data Factory, které jsou používané toocopy data z místního úložiště dat DB2.

Hello následující části obsahují podrobné informace o hello JSON vlastnosti, které jsou používané toodefine hello objekt pro vytváření dat entity, které jsou úložiště dat konkrétní tooa DB2.

## <a name="db2-linked-service-properties"></a>Vlastnosti DB2 propojené služby
Hello následující tabulka uvádí vlastnosti hello JSON, které jsou specifické tooa DB2 propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| **Typ** |Tato vlastnost musí být nastavená příliš**OnPremisesDB2**. |Ano |
| **Server** |Hello název serveru hello DB2. |Ano |
| **databáze** |Hello název databáze hello DB2. |Ano |
| **schéma** |Název Hello hello schématu v databázi hello DB2. Tato vlastnost je malá a velká písmena. |Ne |
| **authenticationType.** |Hello typ ověřování, které je použité tooconnect toohello DB2 databáze. Hello možné hodnoty jsou: anonymní, základní a systému Windows. |Ano |
| **uživatelské jméno** |Název Hello hello uživatelskému účtu, pokud používáte ověřování Basic nebo Windows. |Ne |
| **heslo** |Hello heslo pro uživatelský účet hello. |Ne |
| **gatewayName** |Název Hello hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi DB2. |Ano |

## <a name="dataset-properties"></a>Vlastnosti datové sady
Seznam hello části a vlastnosti, které jsou k dispozici pro definování datové sady, naleznete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Částech, jako například **struktura**, **dostupnosti**a hello **zásad** pro datovou sadu JSON, jsou podobné pro všechny typy datovou sadu (SQL Azure, Azure Blob storage, Azure Table úložiště a tak dále).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. Hello **rámci typeProperties** části datové sady typu **RelationalTable**, která obsahuje datovou sadu hello DB2, má hello následující vlastnost:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| **Název tabulky** |Hello název tabulky hello hello DB2 instance databáze, kterou hello propojená služba odkazuje. Tato vlastnost je malá a velká písmena. |Ne (Pokud hello **dotazu** vlastnost aktivity kopírování typu **RelationalSource** je zadána) |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Seznam oddílů hello a vlastnosti, které jsou k dispozici pro definování aktivity kopírování najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Kopírovat vlastnosti aktivity, jako například **název**, **popis**, **vstupy** tabulky, **výstupy** tabulky, a **zásad**, jsou k dispozici pro všechny typy aktivit. Hello vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivity pro každý typ aktivity. Pro aktivitu kopírování vlastnosti hello lišit v závislosti na typech hello zdroje dat a jímky.

Pro aktivitu kopírování, pokud je zdroj hello typu **RelationalSource** (která zahrnuje DB2), hello následující vlastnosti jsou k dispozici v hello **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| **dotaz** |Hello vlastního dotazu tooread hello data použijte. |Řetězec dotazu SQL. Příklad: `"query": "select * from "MySchema"."MyTable""` |Ne (Pokud hello **tableName** je zadána vlastnost datové sady) |

> [!NOTE]
> Schéma a tabulku názvy rozlišují malá a velká písmena. V příkazu dotazu hello, uzavřete názvy vlastností pomocí "" (dvojité uvozovky). Například:
>
> ```sql
> "query": "select * from "DB2ADMIN"."Customers""
> ```

## <a name="json-example-copy-data-from-db2-tooazure-blob-storage"></a>Příklad JSON: kopírování dat z DB2 tooAzure úložiště objektů Blob
Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Hello příklad ukazuje, jak toocopy data z DB2 databáze tooBlob úložiště. Ale data se dají zkopírovat příliš[všechna podporovaná data ukládat typ jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování objektu pro vytváření dat Azure.

Ukázka Hello má hello následující entity služby Data Factory:

- DB2 propojené služby typu [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)
- Azure Blob storage, propojené služby typu [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)
- Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)
- Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
- A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá hello [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) vlastnosti

Ukázka Hello zkopíruje data z výsledku dotazu v tooan databáze DB2 objektů blob v Azure každou hodinu. Vlastnosti Hello JSON, které se používají v ukázce hello jsou popsané v hello oddíly, které následují definice entity hello.

Jako první krok nainstalujte a nakonfigurujte bránu data gateway. Pokyny naleznete v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

**DB2 propojené služby**

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
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

**Objekt Blob propojená služba Azure storage**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**Vstupní datové sady DB2**

Hello příkladu se předpokládá, že jste vytvořili tabulku v s názvem "MyTable", která má sloupec s názvem "časové razítko" hello časových řad dat DB2.

Hello **externí** vlastnost je nastavena příliš "PRAVDA." Toto nastavení informuje služba Data Factory hello, že tato datová sada je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello. Všimněte si, že hello **typ** vlastnost je nastavena příliš**RelationalTable**.


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
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

**Výstupní datovou sadu objektů Blob v Azure**

Data jsou zapsána nový objekt blob tooa každou hodinu nastavení hello **frekvence** vlastnost příliš "Hodina" a hello **interval** too1 vlastnost. Hello **folderPath** vlastností pro objekt blob hello dynamicky vyhodnotí podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá hello rok, měsíc, den a hodina části hello počáteční čas.

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kanál pro aktivitu kopírování hello**

Hello kanál obsahuje aktivitu kopírování, který je nakonfigurovaný toouse zadaný vstupní a výstupní datové sady a který je naplánované toorun každou hodinu. V definici JSON pro kanál hello hello, hello **zdroj** je typ nastaven příliš**RelationalSource** a hello **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data z tabulky "Objednávky" hello.

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for hello copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
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
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a>Mapování typu pro DB2
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí převody automatické typ z typu toosink typ zdroje pomocí hello následující dvoustupňový přístup:

1. Převést z typu nativní zdroj tooa typ rozhraní .NET
2. Převést z typu nativní podřízený tooa typ rozhraní .NET

Hello následující mapování se používají při aktivitě kopírování převádí hello data z typu DB2 tooa typ rozhraní .NET:

| Typ databáze DB2 | Typ rozhraní .NET framework |
| --- | --- |
| SmallInt |Int16 |
| Integer |Int32 |
| BigInt |Int64 |
| Real |Jeden |
| Double |Double |
| Plovoucí desetinná čárka |Double |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| číselné |Decimal |
| Datum |Data a času |
| Čas |Časový interval |
| časové razítko |Data a času |
| XML |Byte] |
| Char |Řetězec |
| VarChar |Řetězec |
| LongVarChar |Řetězec |
| DB2DynArray |Řetězec |
| Binární |Byte] |
| VarBinary |Byte] |
| LongVarBinary |Byte] |
| Obrázek |Řetězec |
| VarGraphic |Řetězec |
| LongVarGraphic |Řetězec |
| Datový typ CLOB |Řetězec |
| Objekt blob |Byte] |
| DbClob |Řetězec |
| SmallInt |Int16 |
| Integer |Int32 |
| BigInt |Int64 |
| Real |Jeden |
| Double |Double |
| Plovoucí desetinná čárka |Double |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| číselné |Decimal |
| Datum |Data a času |
| Čas |Časový interval |
| časové razítko |Data a času |
| XML |Byte] |
| Char |Řetězec |

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn jak zjistit, toomap sloupců v toocolumns datové sady zdroje hello v datové sadě podřízený hello, [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>Opakovatelných čtení z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Můžete také nakonfigurovat opakování hello **zásad** vlastnost pro datovou sadu toorerun řez, když dojde k chybě. Ujistěte se, který hello stejných dat je pro čtení bez ohledu na to jak řez hello tolikrát, kolikrát je spustit znovu a bez ohledu na to, jak znovu hello řez. Další informace najdete v tématu [Repeatable čte z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
Další informace o klíčových faktorů, které ovlivňují výkon hello aktivity kopírování a způsoby toooptimize výkonu v hello [výkonu kopie aktivity a ladění průvodce](data-factory-copy-activity-performance.md).
