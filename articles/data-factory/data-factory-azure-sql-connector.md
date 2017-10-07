---
title: aaaCopy data do/z Azure SQL Database | Microsoft Docs
description: "Zjistěte, jak toocopy data do/z Azure SQL Database pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: d2ff16191afb028da75699c5e4d0bb310538db0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-database-using-azure-data-factory"></a>Tooand kopírování dat z databáze SQL Azure pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data tooand z databáze SQL Azure. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.  

## <a name="supported-scenarios"></a>Podporované scénáře
Může kopírovat data **z databáze Azure SQL Database** toohello následující úložišť dat:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Data můžete zkopírovat z hello následující úložišť dat **tooAzure SQL Database**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a>Podporované typ ověřování
Konektor služby Azure SQL Database podporuje základní ověřování.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure SQL Database pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **objekt pro vytváření dat**. Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů. 
2. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory. Například pokud kopírování dat z Azure SQL database tooan úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink vašeho účtu úložiště Azure a Azure SQL databáze tooyour data factory. Vlastnosti propojené služby, které jsou specifické tooAzure SQL Database, najdete v části [propojené vlastnosti služby](#linked-service-properties) části. 
3. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello. A vytvořte jinou datovou sadu toospecify hello SQL tabulkou v hello Azure SQL database, která obsahuje hello daty zkopírovanými z úložiště objektů blob hello. Vlastnosti datové sady, které jsou specifické tooAzure Data Lake Store, naleznete v části [vlastnosti datové sady](#dataset-properties) části.
4. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlSink jako jímku pro aktivitu kopírování hello. Podobně pokud zkopírujete z Azure SQL Database tooAzure úložiště objektů Blob, použijte SqlSource a BlobSink v aktivitě kopírování hello. Vlastnosti aktivity kopírování, které jsou specifické tooAzure SQL Database, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části. Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do/z Azure SQL Database, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-sql-database) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure SQL Database: 

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Azure SQL propojená služba propojuje služby Azure SQL database tooyour data factory. Hello následující tabulka obsahuje popis JSON elementy konkrétní tooAzure SQL propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **azuresqldatabase.** |Ano |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Database instance. Podporováno je pouze základní ověřování. |Ano |

> [!IMPORTANT]
> Konfigurace [Firewall databáze Azure SQL](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) hello databázový server příliš[povolit serveru hello tooaccess Azure Services](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Kromě toho pokud kopírujete data tooAzure databáze SQL z mimo Azure včetně z místního zdroje dat se brána objekt pro vytváření dat, nakonfigurujte odpovídající rozsah IP adres pro hello počítač, který odesílá data tooAzure databáze SQL.

## <a name="dataset-properties"></a>Vlastnosti datové sady
toospecify toorepresent datové sady vstupních nebo výstupních dat v databázi Azure SQL, nastavte vlastnost typu hello hello datovou sadu, která: **AzureSqlTable**. Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Azure SQL, propojené služby.  

Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. Hello **rámci typeProperties** části pro datovou sadu hello typu **AzureSqlTable** má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název hello tabulku nebo zobrazení hello Azure SQL Database instance, kterou propojená služba odkazuje. |Ano |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

> [!NOTE]
> Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.

Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pokud přesouváte data z databáze Azure SQL, nastavíte typ zdroje hello v aktivitě kopírování hello příliš**SqlSource**. Podobně pokud přesouváte data tooan Azure SQL database, nastavíte typ jímky hello v aktivitě kopírování hello příliš**SqlSink**. Tato část obsahuje seznam vlastností, které jsou podporované SqlSource a SqlSink.

### <a name="sqlsource"></a>SqlSource
Při aktivitě kopírování, pokud je zdroj hello typu **SqlSource**, hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| sqlReaderQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Příklad: `select * from MyTable`. |Ne |
| sqlReaderStoredProcedureName |Název hello uložené procedury, která čte data z hello zdrojové tabulky. |Název hello uložené procedury. Hello poslední příkaz jazyka SQL musí být příkaz SELECT v hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |

Pokud hello **sqlReaderQuery** je zadán pro hello SqlSource, hello aktivity kopírování spouští tento dotaz hello Azure SQL Database zdrojová tooget hello data. Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definované v části struktura hello hello sady dat JSON jsou použité toobuild dotazu (`select column1, column2 from mytable`) toorun proti hello Azure SQL Database. Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.

> [!NOTE]
> Při použití **sqlReaderStoredProcedureName**, stále potřebujete toospecify hodnotu pro hello **tableName** vlastnost v datové sadě hello JSON. Neexistují žádné ověření, ale adresovat této tabulky.
>
>

### <a name="sqlsource-example"></a>Příklad SqlSource

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

**definice Hello uložené procedury:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqlsink"></a>SqlSink
**SqlSink** podporuje hello následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| writeBatchTimeout |Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: "00: 30:00" (30 minut). |Ne |
| writeBatchSize |Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello. |Celé číslo (počet řádků) |Ne (výchozí: 10000) |
| sqlWriterCleanupScript |Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez. Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy). |Příkaz dotazu. |Ne |
| sliceIdentifierColumnName |Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit. Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy). |Název sloupce sloupce s datovým typem binary(32). |Ne |
| sqlWriterStoredProcedureName |Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello. |Název hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |
| sqlWriterTableType |Zadejte toobe tabulku typu název používá v hello uložené procedury. Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky. Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty. |Zadejte název tabulky. |Ne |

#### <a name="sqlsink-example"></a>Příklad SqlSink

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-sql-database"></a>Příklady JSON pro kopírování data tooand z databáze SQL
Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak toocopy tooand dat z Azure SQL Database a Azure Blob Storage. Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

### <a name="example-copy-data-from-azure-sql-database-tooazure-blob"></a>Příklad: Kopírování dat z Azure SQL Database tooAzure objektů Blob
Hello stejné definuje hello následující entity služby Data Factory:

1. Propojené služby typu [azuresqldatabase](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [objektů Blob v Azure](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data časové řady (hodinový, denní, atd.) z tabulky v objektu blob tooa databáze Azure SQL každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.  

**Azure SQL Database propojené služby:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
V tématu hello [propojená služba Azure SQL](#linked-service) části hello seznamu vlastností podporuje tuto propojenou službu.

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
V tématu hello [objektů Blob v Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) článku hello seznamu vlastností podporuje tuto propojenou službu.


**Azure SQL vstupní datové sady:**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Azure SQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.

Nastavení "externí": "PRAVDA" informuje služby Azure Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

V tématu hello [vlastnosti typu datové sady Azure SQL](#dataset) části hello seznamu vlastností nepodporuje tento typ dataset.  

**Azure Blob výstupní datovou sadu:**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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
V tématu hello [vlastnosti typu datovou sadu objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) části hello seznamu vlastností nepodporuje tento typ dataset.  

**Aktivita kopírování v kanálu s SQL zdrojový a podřízený objekt Blob:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
V příkladu hello **sqlReaderQuery** pro hello SqlSource je zadána. Aktivita kopírování Hello spouští tento dotaz hello Azure SQL Database zdrojová tooget hello data. Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello hello sady dat JSON jsou použité toobuild toorun dotaz proti hello Azure SQL Database. Například: `select column1, column2 from mytable`. Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.

V tématu hello [zdroje Sql](#sqlsource) části a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem SqlSource a BlobSink.

### <a name="example-copy-data-from-azure-blob-tooazure-sql-database"></a>Příklad: Kopírování dat z Azure Blob tooAzure databáze SQL
Ukázka Hello definuje hello následující entity služby Data Factory:  

1. Propojené služby typu [azuresqldatabase](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlSink](#copy-activity-properties).

Ukázka Hello zkopíruje data časové řady (hodinový, denní, atd.) z Azure blob tooa tabulky v databázi Azure SQL každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Azure SQL propojené služby:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
V tématu hello [propojená služba Azure SQL](#linked-service) části hello seznamu vlastností podporuje tuto propojenou službu.

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
V tématu hello [objektů Blob v Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) článku hello seznamu vlastností podporuje tuto propojenou službu.


**Azure vstupní datovou sadu objektu Blob:**

Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas. "externí": "PRAVDA" nastavení informuje hello služba Data Factory, tato tabulka je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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
V tématu hello [vlastnosti typu datovou sadu objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) části hello seznamu vlastností nepodporuje tento typ dataset.

**Azure SQL Database výstupní datovou sadu:**

Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v Azure SQL. Vytvoření tabulky hello v Azure SQL s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello. Přidávání řádků tabulky toohello každou hodinu.

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
V tématu hello [vlastnosti typu datové sady Azure SQL](#dataset) části hello seznamu vlastností nepodporuje tento typ dataset.

**Aktivita kopírování v kanálu s Blob zdroj a jímka SQL:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
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
V tématu hello [jímku Sql](#sqlsink) části a [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem SqlSink a BlobSource.

## <a name="identity-columns-in-hello-target-database"></a>Sloupce identity v hello cílová databáze
Tato část poskytuje příklad pro kopírování dat ze zdrojové tabulky bez identity sloupec tooa cílové tabulky se sloupcem identity.

**Zdrojová tabulka:**

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Cílové tabulky:**

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
Všimněte si, že hello cílová tabulka obsahuje sloupec identity.

**Definice JSON datové sady zdroje**

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Cílový definici JSON datové sady**

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }    
}
```

Všimněte si, že jako zdrojové a cílové tabulky jiné schéma (cíl má sloupec s identitou). V tomto scénáři budete potřebovat toospecify **struktura** vlastnost v definici datové sady cíl hello, který neobsahuje sloupec identity hello.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Volání uložené procedury jímku SQL
Příklad volání uložené procedury z jímku SQL při aktivitě kopírování kanálu, naleznete v části [vyvolat uloženou proceduru SQL jímka v aktivitě kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md) článku. 

## <a name="type-mapping-for-azure-sql-database"></a>Mapování typu pro databázi SQL Azure
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesunu data tooand z databáze Azure SQL Database, hello následující mapování se používají z typu too.NET typ SQL a naopak. mapování Hello je stejný jako hello mapování datového typu aplikace SQL Server pro technologii ADO.NET.

| Typ databázového stroje SQL Server | Typ rozhraní .NET framework |
| --- | --- |
| bigint |Int64 |
| Binární |Byte] |
| Bit |Logická hodnota |
| Char |Řetězec, Char] |
| Datum |Data a času |
| Data a času |Data a času |
| datetime2 |Data a času |
| Datový typ DateTimeOffset |Datový typ DateTimeOffset |
| Decimal |Decimal |
| Atribut FILESTREAM (varbinary(max)) |Byte] |
| Plovoucí desetinná čárka |Double |
| Bitové kopie |Byte] |
| celá čísla |Int32 |
| peníze |Decimal |
| nchar |Řetězec, Char] |
| ntext |Řetězec, Char] |
| číselné |Decimal |
| nvarchar |Řetězec, Char] |
| skutečné |Jeden |
| ROWVERSION |Byte] |
| smalldatetime |Data a času |
| smallint |Int16 |
| Smallmoney |Decimal |
| SQL_VARIANT |Objekt * |
| Text |Řetězec, Char] |
| time |Časový interval |
| časové razítko |Byte] |
| tinyint |Bajtů |
| Typ UniqueIdentifier |Identifikátor GUID |
| varbinary |Byte] |
| varchar |Řetězec, Char] |
| xml |XML |

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Opakovatelných kopie
Při kopírování dat tooSQL databáze serveru, aktivity kopírování hello připojí tabulky jímky toohello data ve výchozím nastavení. Místo toho najdete v části tooperform UPSERT [tooSqlSink opakovatelných zápisu](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) článku. 

Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
