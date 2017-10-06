---
title: "aaaMove data z tabulky webové pomocí Azure Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z tabulky v webové stránce pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e52216305583ebbe71ed896522f361bb22f01278
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Přesun dat z webové zdroje tabulky pomocí Azure Data Factory
Tento článek popisuje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z tabulky v tooa webové stránky podporované úložiště dat jímky. Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s kopie aktivity a hello seznamu úložiště dat, které jsou podporované jako zdroje nebo jímky.

Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z datové webové tabulky tooother ukládá, ale není přesouvání dat od ostatních dat ukládá cíl tabulky tooa webu.

> [!IMPORTANT]
> Tento konektor webové aktuálně podporuje pouze extrakce obsahu tabulky z stránku HTML. tooretrieve data z koncového bodu protokolu HTTP/s, používat [HTTP konektor](data-factory-http-connector.md) místo.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API. 

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello. 
- Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z tabulky web, naleznete v tématu [JSON příklad: kopírování dat z webové tabulky tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa webové tabulce:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooWeb propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **Web** |Ano |
| URL |Adresa URL toohello webové zdroje |Ano |
| authenticationType. |Anonymní. |Ano |

### <a name="using-anonymous-authentication"></a>Pomocí anonymní ověřování

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **WebTable** má následující vlastnosti hello

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |Typ hello datovou sadu. musí být nastaven příliš**WebTable** |Ano |
| Cesta |Na relativní adresa URL toohello prostředek, který obsahuje tabulku hello. |Ne. Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby. |
| Index |index Hello hello tabulky v hello prostředku. V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části kroky toogetting index tabulky v stránku HTML. |Ano |

**Příklad:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

V současné době po hello zdroj v aktivitě kopírování typu **WebSource**, jsou podporovány žádné další vlastnosti.


## <a name="json-example-copy-data-from-web-table-tooazure-blob"></a>Příklad JSON: kopírování dat z webové tabulky tooAzure objektů Blob
Následující ukázka ukazuje Hello:

1. Propojené služby typu [webové](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [WebTable](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [WebSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z tabulky tooan webové objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Následující ukázka Hello ukazuje, jak toocopy dat z tabulky tooan Web Azure blob. Ale data se dají zkopírovat přímo tooany hello jímky stanovené v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek pomocí hello aktivitu kopírování v Azure Data Factory.

**Webové propojená služba** tento příklad používá hello webové propojená služba s anonymní ověřování. V tématu [webové propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

**Propojená služba Azure Storage**

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

**Vstupní datové sady WebTable** nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v hello služby data factory.

> [!NOTE]
> V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části kroky toogetting index tabulky v stránku HTML.  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Výstupní datovou sadu objektů Blob v Azure**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



**Kanál s aktivitou kopírování**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**WebSource** a **podřízený** je typ nastaven příliš**BlobSink**.

V tématu [vlastnosti typu WebSource](#copy-activity-type-properties) hello seznam vlastnostech podporovaných zprostředkovatelem hello WebSource.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
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

## <a name="get-index-of-a-table-in-an-html-page"></a>Na stránce HTML získat index tabulky.
1. Spusťte **Excel 2016** a přepínač toohello **Data** kartě.  
2. Klikněte na tlačítko **nový dotaz** na panelu nástrojů hello bodu příliš**z jiných zdrojů** a klikněte na tlačítko **z webové**.

    ![Power Query nabídky](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. V hello **z webové** dialogovém okně zadejte **URL** , kterou použijete v propojené službě JSON (například: https://en.wikipedia.org/wiki/) společně s cesty zadáte pro datovou sadu hello (například: AFI % 27s_100_Years... 100_Movies) a klikněte na tlačítko **OK**.

    ![Z webové dialogového okna](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    Adresa URL použitá v tomto příkladu: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Pokud se zobrazí **přístup k webovému obsahu** dialogové okno, vyberte hello vpravo **URL**, **ověřování**a klikněte na tlačítko **Connect**.

   ![Přístup k dialogové okno webového obsahu](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Klikněte na tlačítko **tabulky** položky v hello stromové zobrazení toosee obsahu z tabulky hello a pak klikněte na **upravit** tlačítko dole v hello.  

   ![Dialogové okno Navigátor](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. V hello **Editor dotazů** okně klikněte na tlačítko **Upřesnit** tlačítka na panelu nástrojů hello.

    ![Tlačítko Rozšířené editoru](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. V dialogové okno Upřesnit hello, hello číslo vedle příliš "Zdroj" je hello index.

    ![Rozšířené Editor - indexu](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Pokud používáte Excel 2013, použijte [Microsoft Power Query pro Excel](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index. V tématu [Connect tooa webové stránky](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) článku. Hello kroky jsou podobné, pokud používáte [Microsoft Power BI pro plochu](https://powerbi.microsoft.com/desktop/).

> [!NOTE]
> toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
