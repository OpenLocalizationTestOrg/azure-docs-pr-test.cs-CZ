---
title: aaaMove data do/z Azure Cosmos DB | Microsoft Docs
description: "Zjistěte, jak přesunout data do nebo z kolekce Azure Cosmos DB pomocí Azure Data Factory"
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: bd23ce4e004a972ce6f3e4165cfdea4f0c18fecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-cosmos-db-using-azure-data-factory"></a>Přesunout data tooand z databáze Cosmos Azure pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove dat z Azure DB Cosmos (DocumentDB rozhraní API). Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello. 

Data můžete zkopírovat z jakéhokoli podporované zdroje dat ukládání tooAzure Cosmos DB nebo z Azure Cosmos DB tooany podporované jímku dat úložiště. Seznam úložišť dat jako zdroje nebo jímky podporované aktivitou kopírování hello najdete v tématu hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. 

> [!IMPORTANT]
> Konektor Azure Cosmos DB podporují pouze DocumentDB API.

data toocopy jako-je do nebo ze soubory JSON nebo jiné Cosmos DB kolekce najdete v části [dokumentů JSON importu a exportu](#importexport-json-documents).

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Cosmos DB pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo z databáze Cosmos naleznete v části [JSON příklady](#json-examples) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooCosmos DB: 

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis pro konkrétní tooAzure elementy JSON Cosmos DB propojené služby.

| **Vlastnost** | **Popis** | **Požadované** |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **DocumentDb** |Ano |
| připojovací řetězec |Zadejte informace potřebné tooconnect tooAzure Cosmos DB databáze. |Ano |

Příklad:

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datových sad naleznete toohello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly jako struktura, dostupnosti a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello části pro datovou sadu hello typu **DocumentDbCollection** má následující vlastnosti hello.

| **Vlastnost** | **Popis** | **Požadované** |
| --- | --- | --- |
| Název_kolekce |Název hello kolekce dokumentů Cosmos DB. |Ano |

Příklad:

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Schéma službou Data Factory
Pro data bez schémat úložiště, jako je například Azure Cosmos DB odvodí hello služba Data Factory hello schéma v jednom z následujících způsobů hello:  

1. Pokud zadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, hello služba Data Factory ctí tato struktura jako hello schématu. V takovém případě Pokud řádek neobsahuje hodnotu pro sloupec, bude poskytnuta hodnota null pro ni.
2. Pokud nezadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, hello služba Data Factory odvodí hello schématu pomocí hello první řádek v datech hello. V takovém případě pokud první řádek hello neobsahuje úplnou schématu hello, některé sloupce budou chybět v hello výsledek operace kopírování.

Proto pro zdroje dat bez schémat, hello osvědčeným postupem je toospecify hello strukturu dat pomocí hello **struktura** vlastnost.

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity naleznete toohello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

> [!NOTE]
> Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.

Vlastnosti dostupné v rámci typeProperties části hello hello aktivit na hello lišit druhé straně, každý typ aktivity a v případě aktivitě kopírování se liší v závislosti na typech hello zdrojů a jímky.

V případě aktivitu kopírování, pokud je zdroj typu **DocumentDbCollectionSource** hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:

| **Vlastnost** | **Popis** | **Povolené hodnoty** | **Požadované** |
| --- | --- | --- | --- |
| query |Zadejte hello dotazu tooread data. |Řetězec nepodporuje Azure Cosmos DB dotazu. <br/><br/>Příklad:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Ne <br/><br/>Pokud není zadaný, hello příkaz jazyka SQL, který se spustí:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Je vnořený tooindicate speciální znak, který hello dokumentu |Libovolný znak. <br/><br/>Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury. Azure Data Factory umožňuje hierarchie toodenote uživatele prostřednictvím nestingSeparator, což je "." v hello výše příklady. S oddělovačem hello aktivity kopírování hello vygeneruje hello "Name" objekt s tří podřízených elementů první, střední a poslední, podle too"Name.First", "Name.Middle" a "Name.Last" v hello Definice tabulky. |Ne |

**DocumentDbCollectionSink** podporuje hello následující vlastnosti:

| **Vlastnost** | **Popis** | **Povolené hodnoty** | **Požadované** |
| --- | --- | --- | --- |
| nestingSeparator |Je potřeba speciálního znaku v hello zdrojový sloupec název tooindicate, který vnořených dokumentů. <br/><br/>Například výše: `Name.First` ve výstupu hello tabulky vytváří hello strukturu JSON v dokumentu Cosmos DB hello:<br/><br/>"Název": {<br/>    "První": "Jan"<br/>}, |Znak, který je použité tooseparate vnořených úrovní.<br/><br/>Výchozí hodnota je `.` (tečka). |Znak, který je použité tooseparate vnořených úrovní. <br/><br/>Výchozí hodnota je `.` (tečka). |
| writeBatchSize |Počet paralelní požadavků tooAzure Cosmos DB služby toocreate dokumenty.<br/><br/>Při kopírování dat z databáze Cosmos pomocí této vlastnosti lze optimalizovat výkon hello. Lepšího výkonu můžete očekávat, když zvýšíte writeBatchSize, protože se odesílají další paralelní požadavky tooCosmos DB. Ale budete potřebovat tooavoid omezení, který lze vyvolat hello chybová zpráva: "Požadavků je velká".<br/><br/>Omezení je určeno podle počtu faktorů, včetně velikosti dokumentů, počet podmínky v dokumentech, indexování zásad cílovou kolekci, atd. Pro operace kopírování, můžete použít lepší hello toohave kolekce (např. S3) většina propustnost, které jsou k dispozici (2 500 žádostí jednotek za sekundu). |Integer |Ne (výchozí: 5) |
| writeBatchTimeout |Doba pro operaci toocomplete hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: "00: 30:00" (30 minut). |Ne |

## <a name="importexport-json-documents"></a>Dokumenty JSON importu a exportu
Pomocí tohoto konektoru Cosmos DB, můžete snadno

* Importujte dokumenty JSON z různých zdrojů do Cosmos databáze, včetně objektů Blob v Azure, Azure Data Lake, v místním systému souborů nebo jiných úložišť na základě souborů podporovaných službou Azure Data Factory.
* Exportujte dokumenty JSON ze collecton Cosmos databáze do různých úložišť na základě souborů.
* Migrace dat mezi dvě kolekce Cosmos DB jako-je.

tooachieve takové schématu bez ohledu na Kopírovat, 
* Při použití Průvodce kopírováním, zkontrolujte hello **"exportovat jako-tooJSON soubory nebo kolekce Cosmos DB"** možnost.
* Když pomocí úpravy JSON, nezadávejte část "struktura" hello v datových sad Cosmos DB ani vlastnost "nestingSeparator" na Cosmos DB zdroj/jímka v aktivitě kopírování. tooimport z / exportovat soubory tooJSON, v datové sadě úložiště hello souboru zadejte typ formátu jako "JsonFormat", "filePattern" Konfigurace a přeskočit nastavení formátu hello rest, najdete v části [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format) části na podrobnosti.

## <a name="json-examples"></a>Příklady JSON
Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak toocopy tooand data z databáze Cosmos Azure a Azure Blob Storage. Však můžete zkopírovat data **přímo** z jakéhokoli zdroje tooany hello z hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

## <a name="example-copy-data-from-azure-cosmos-db-tooazure-blob"></a>Příklad: Kopírování dat z Azure Cosmos DB tooAzure objektů Blob
znázorňuje následující ukázka Hello:

1. Propojené služby typu [DocumentDb](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [DocumentDbCollectionSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data v Azure Cosmos DB tooAzure objektů Blob. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Azure Cosmos DB propojené služby:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Propojená služba Azure Blob storage:**

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
**Azure Documentdb vstupní datové sady:**

Ukázka Hello předpokládá, že máte kolekci s názvem **osoba** v databázi Azure Cosmos DB.

Nastavení "externí": "PRAVDA" a zadání externalData informace o zásadách hello Azure Data Factory služby Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Azure Blob výstupní datovou sadu:**

Data jsou nový objekt blob zkopírovaný tooa každou hodinu s hello cesty pro objekt blob hello odrážející konkrétní datum a čas hello s rozlišením hodinu.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Ukázka dokumentu JSON v hello osoba kolekce v databázi Cosmos DB:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
Cosmos DB podporuje dotazování dokumentů pomocí SQL jako syntaxe přes hierarchické dokumentů JSON.

Příklad: 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

Následující Hello kanál kopíruje data z hello osoba kolekce v hello tooan databáze Azure Cosmos DB objektů blob v Azure. Jako součást hello kopie aktivity hello nebyly zadány vstupní a výstupní datové sady.  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-tooazure-cosmos-db"></a>Příklad: Kopírování dat z Azure Blob tooAzure Cosmos DB 
znázorňuje následující ukázka Hello:

1. Propojené služby typu [DocumentDb](#azure-documentdb-linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).

Ukázka Hello zkopíruje data z Azure blob tooAzure Cosmos DB. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Propojená služba Azure Blob storage:**

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
**Azure Cosmos DB propojené služby:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Azure vstupní datovou sadu objektu Blob:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Azure Cosmos DB výstupní datovou sadu:**

Ukázka Hello zkopíruje data tooa kolekci s názvem "Osoba".

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Následující Hello kanál kopíruje data z Azure Blob toohello osoba kolekce v hello Cosmos DB. Jako součást hello kopie aktivity hello nebyly zadány vstupní a výstupní datové sady.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Pokud hello ukázkových objektů blob vstup je jako

```
1,John,,Doe
```
Výstup hello JSON v Cosmos DB pak bude jako:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury. Azure Data Factory umožňuje hierarchie toodenote uživatele prostřednictvím **nestingSeparator**, což je "." v tomto příkladu. S oddělovačem hello aktivity kopírování hello vygeneruje hello "Name" objekt s tří podřízených elementů první, střední a poslední, podle too"Name.First", "Name.Middle" a "Name.Last" v hello Definice tabulky.

## <a name="appendix"></a>Příloha
1. **Otázka:** hello aktivity kopírování podporu aktualizace existující záznamy?

    **Odpověď:** ne.
2. **Otázka:** jak již nemá opakování ke kopírování tooAzure Cosmos DB pozornosti s zkopírovat záznamy?

    **Odpověď:** Pokud záznamy na pole "ID" a operace kopírování hello pokusí tooinsert záznam s hello stejné ID operace kopírování hello vrátí chybu.  
3. **Otázka:** podporuje služby Data Factory [rozsah nebo dělení dat na základě hodnoty hash](../documentdb/documentdb-partition-data.md)?

    **Odpověď:** ne.
4. **Otázka:** můžete zadat více než jednu kolekci Azure Cosmos DB pro tabulku?

    **Odpověď:** ne. V tuto chvíli je možné zadat pouze jednu kolekci.

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
