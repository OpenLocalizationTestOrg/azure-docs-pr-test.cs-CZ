---
title: "index tooSearch aaaPush dat pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom toopush data tooAzure indexu vyhledávání pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: f2d973d0a2c24d6448e2d59e37e24503aa433018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="push-data-tooan-azure-search-index-by-using-azure-data-factory"></a>Push indexu Azure Search tooan dat pomocí Azure Data Factory
Tento článek popisuje, jak úložiště toouse hello aktivity kopírování toopush dat z podporovaných zdrojových dat tooAzure indexu vyhledávání. Podporované zdrojové úložiště dat jsou uvedené v hello zdrojový sloupec %{sourceproperty/ hello [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a kombinace podporované datové úložiště.

## <a name="enabling-connectivity"></a>Povolení připojení
tooallow služba Data Factory připojení tooan místní data store, nainstalujte Brána pro správu dat ve vašem místním prostředí. Bránu můžete nainstalovat na hello stejný počítač, který je hostitelem hello zdrojová data uložit nebo na samostatný počítač tooavoid, neslučitelných pro prostředky s hello data uložit.

Brána pro správu dat se připojuje místní datové zdroje toocloud služby způsobem, zabezpečení a správě. V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o Brána pro správu dat najdete v článku.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který by vložil data ze zdroje dat úložiště tooAzure vyhledávání indexu pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data tooAzure vyhledávání index, naleznete v tématu [JSON příklad: kopírování dat z indexu vyhledávání tooAzure místní systém SQL Server](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure indexu vyhledávání:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Hello následující tabulka obsahuje popis JSON prvky, které jsou konkrétní toohello Azure Search propojené služby.

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| type | vlastnost typu Hello musí být nastavena na: **AzureSearch**. | Ano |
| Adresa URL | Adresa URL pro hello služby Azure Search. | Ano |
| key | Klíč správce pro hello služby Azure Search. | Ano |

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu. Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu. rámci typeProperties Hello část datové sady hello typu **AzureSearchIndex** má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| type | musí být nastavena vlastnost typu Hello příliš**AzureSearchIndex**.| Ano |
| indexName | Název indexu Azure Search hello. Objekt pro vytváření dat nevytváří hello index. Hello index musí existovat ve službě Azure Search. | Ano |


## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a různé zásady jsou dostupné pro všechny typy aktivit. Zatímco se každý typ aktivity lišit podle vlastnosti dostupné v rámci typeProperties části hello. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pro aktivitu kopírování, po hello podřízený typu hello **AzureSearchIndexSink**, hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Určuje, zda toomerge nebo nahradit, když dokumentu již existuje v indexu hello. V tématu hello [WriteBehavior vlastnost](#writebehavior-property).| Merge (výchozí)<br/>Odeslat| Ne |
| writeBatchSize | Ukládání dat do indexu Azure Search hello, když velikost vyrovnávací paměti hello dosáhne writeBatchSize. V tématu hello [WriteBatchSize vlastnost](#writebatchsize-property) podrobnosti. | 1 too1 000. Výchozí hodnota je 1 000. | Ne |

### <a name="writebehavior-property"></a>Vlastnost WriteBehavior
Při zápisu dat AzureSearchSink upserts. Jinými slovy při zápisu dokumentu, pokud klíč dokumentu hello již existuje v indexu Azure Search hello, Azure Search aktualizuje stávající dokument hello místo vyvolání k výjimce konflikt.

Hello AzureSearchSink poskytuje hello následující dvě upsert chování (pomocí AzureSearch SDK):

- **Sloučení**: kombinovat všechny sloupce hello v hello nový dokument s hello existující jeden. Pro sloupce s hodnotou null v hello nový dokument je hodnota hello v hello existující jeden zachovaná.
- **Nahrát**: hello hello nový dokument nahradí existující. Pro sloupce, které nebyly zadány v hello nový dokument hello hodnota je nastavena toonull, zda je jinou hodnotu než null v hello existujícího dokumentu nebo ne.

Hello výchozí chování je **sloučení**.

### <a name="writebatchsize-property"></a>Vlastnost WriteBatchSize
Služba Azure Search podporuje zápis dokumentů jako dávku. Batch může obsahovat 1 too1 000 akce. Akce zpracovává jeden dokument tooperform hello nahrávání či sloučení operaci.

### <a name="data-type-support"></a>Podpora datového typu
Hello následující tabulka určuje, zda typ dat. Azure Search je podporováno, nebo ne.

| Azure Search datový typ | Podporované ve službě Azure Search jímka |
| ---------------------- | ------------------------------ |
| Řetězec | Ano |
| Int32 | Ano |
| Int64 | Ano |
| Double | Ano |
| Logická hodnota | Ano |
| DataTimeOffset | Ano |
| Pole řetězců | N |
| GeographyPoint | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-tooazure-search-index"></a>Příklad JSON: kopírování dat z indexu vyhledávání tooAzure místní systém SQL Server

Následující ukázka ukazuje Hello:

1.  Propojené služby typu [AzureSearch](#linked-service-properties).
2.  Propojené služby typu [onpremisessqlserver](data-factory-sqlserver-connector.md#linked-service-properties).
3.  Vstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4.  Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSearchIndex](#dataset-properties).
4.  A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) a [AzureSearchIndexSink](#copy-activity-properties).

Ukázka Hello kopíruje data časové řady každou hodinu z indexu Azure Search tooan databáze místní systém SQL Server. Hello vlastnostech JSON použitých v této ukázce jsou popsané v části následující ukázky hello.

Jako první krok instalační program hello Brána pro správu dat na místním počítači. Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

**Azure Search propojené služby:**

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

**Služba SQL Server propojené**

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

**Vstupní datové sady SQL Server**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v systému SQL Server a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady. Můžete dotazovat přes více tabulek v rámci hello stejnou databázi pomocí jednu datovou sadu, ale jedné tabulky musí být použita pro typeProperty tableName hello datovou sadu.

Nastavení "externí": "PRAVDA" informuje služba Data Factory tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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

**Služba Azure Search výstupní datovou sadu:**

Hello ukázka kopie dat tooan indexu Azure Search s názvem **produkty**. Objekt pro vytváření dat nevytváří hello index. tootest hello ukázkové, vytvořte index s tímto názvem. Vytvoření indexu Azure Search hello s hello stejný počet sloupců jako vstupní datové sady hello. Nové položky se přidají indexu Azure Search toohello každou hodinu.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

**Aktivita kopírování v kanálu s SQL zdroj a jímka indexu Azure Search:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**AzureSearchIndexSink**. Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

Pokud kopírujete data z jiného úložiště dat cloudu Azure Search, `executionLocation` vlastnost je povinná. Hello následující fragment kódu JSON ukazuje změnu hello potřeba v rámci aktivity kopírování `typeProperties` jako příklad. Zkontrolujte [kopírování dat mezi úložišti dat cloudu](data-factory-data-movement-activities.md#global) části Podporované hodnoty a další podrobnosti.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a>Kopírování ze zdrojového cloudu
Pokud kopírujete data z jiného úložiště dat cloudu Azure Search, `executionLocation` vlastnost je povinná. Hello následující fragment kódu JSON ukazuje změnu hello potřeba v rámci aktivity kopírování `typeProperties` jako příklad. Zkontrolujte [kopírování dat mezi úložišti dat cloudu](data-factory-data-movement-activities.md#global) části Podporované hodnoty a další podrobnosti.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

Můžete také mapovat sloupců z toocolumns datové sady zdroje z podřízený datovou sadu v definici aktivity kopírování hello. Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Výkon a ladění  
V tématu hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) a různé způsoby toooptimize ho.

## <a name="next-steps"></a>Další kroky
V tématu hello následující články:

* [Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.
