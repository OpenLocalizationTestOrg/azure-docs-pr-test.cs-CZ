---
title: "aaaMove data ze služby Salesforce pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data ze služby Salesforce pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: c1bde2a333f5a3c0a995eb8c13ecf585132888b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Přesun dat ze služby Salesforce pomocí Azure Data Factory
Tento článek popisuje, jak můžete použít aktivitu kopírování v Azure data factory toocopy data ze služby Salesforce tooany datové úložiště, které se má zobrazit pod hello podřízený sloupec v hello [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a kombinace podporované datové úložiště.

Azure Data Factory v současné době podporuje pouze přesun dat ze služby Salesforce příliš[podporovanými úložišti dat podřízený](data-factory-data-movement-activities.md#supported-data-stores-and-formats), ale nepodporuje přesunutí dat z jiných tooSalesforce ukládá data.

## <a name="supported-versions"></a>Podporované verze
Tento konektor podporuje následujících edice Salesforce hello: Developer Edition, edice Professional, Enterprise Edition nebo neomezená Edition. A podporuje kopírování z výroby, izolovaný prostor a vlastní doménu služby Salesforce.

## <a name="prerequisites"></a>Požadavky
* Musí být povoleno oprávnění rozhraní API. V tématu [povolení přístup pomocí rozhraní API v Salesforce sadou oprávnění?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* toocopy data ze služby Salesforce tooon místní datová úložiště, musíte mít alespoň 2.0 brány správy dat ve vašem prostředí místní instalace.

## <a name="salesforce-request-limits"></a>Omezení žádostí o služby Salesforce
Služba Salesforce má limity pro celkový počet žádostí o rozhraní API a souběžných žádostí o rozhraní API. Všimněte si hello následující body:

- Pokud hello počet souběžných požadavků překročí hello limit, dojde k omezení šířky pásma a zobrazí se náhodné chyby.
- Pokud hello celkový počet požadavků překračuje hello limit, hello účtu Salesforce se zablokuje 24 hodin.

V obou případech se může zobrazit i chyba "REQUEST_LIMIT_EXCEEDED" hello. Najdete v části hello "API omezení požadavků" v hello [Salesforce vývojáře omezení](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) článku.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data ze služby Salesforce pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data ze služby Salesforce, naleznete v tématu [JSON příklad: kopírování dat ze služby Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooSalesforce: 

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON prvky, které jsou specifické toohello Salesforce propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **Salesforce**. |Ano |
| environmentUrl | Zadejte adresu URL služby Salesforce instanci hello. <br><br> -Výchozí hodnota je "https://login.salesforce.com". <br> -toocopy data z izolovaného prostoru, zadejte "https://test.salesforce.com". <br> -toocopy data z vlastní doménu, zadejte například "https://[domain].my.salesforce.com". |Ne |
| uživatelské jméno |Zadejte uživatelské jméno pro hello uživatelský účet. |Ano |
| heslo |Zadejte heslo pro uživatelský účet hello. |Ano |
| securityToken |Zadejte token zabezpečení pro hello uživatelský účet. V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny tooreset nebo získat token zabezpečení. Obecně platí, najdete v části toolearn o tokeny zabezpečení [zabezpečení a hello rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ano |

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL, objektů blob v Azure, Azure table atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady hello typu **RelationalTable** má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v Salesforce. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |

> [!IMPORTANT]
> Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, jako je název, popis, vstupní a výstupní tabulky a jsou dostupné pro všechny typy aktivit různé zásady.

Hello vlastnosti, které jsou k dispozici v rámci typeProperties části hello aktivit na hello hello druhé straně, pomocí jednotlivých typů aktivity se liší. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Při aktivitě kopírování, pokud je zdroj hello hello typu **RelationalSource** (která zahrnuje Salesforce), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Dotaz SQL 92 nebo [Salesforce objektu dotazu jazyka (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) dotazu. Například `select * from MyTable__c`. |Ne (Pokud hello **tableName** z hello **datovou sadu** je zadána) |

> [!IMPORTANT]
> Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Typy dotazů
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Načítání dat pomocí where klauzule ve sloupci data a času
Při zadejte hello SOQL nebo SQL dotaz, platím pozornost toohello rozdíl formátu data a času. Například:

* **Ukázka SOQL**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **Ukázka SQL**:
    * **Pomocí kopírování Průvodce toospecify hello dotazu:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Pomocí JSON úpravou dotazu hello toospecify (char vyhnuli správně):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Načítání dat ze sestavy služby Salesforce
Ze sestavy služby Salesforce můžete data načíst zadáním dotazu jako `{call "<report name>"}`, např. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Načítání odstranit záznamy z funkce Koš služby Salesforce
tooquery hello logicky odstranit záznamy z funkce Koš služby Salesforce, můžete zadat **"IsDeleted = 1"** v dotazu. Například:

* Zadejte tooquery pouze hello odstranit záznamy "vybrat * z MyTable__c **kde IsDeleted = 1**"
* tooquery všechny hello záznamů včetně existující hello a hello odstranit, zadejte "vybrat * z MyTable__c **kde IsDeleted = 0 nebo IsDeleted = 1**"

## <a name="json-example-copy-data-from-salesforce-tooazure-blob"></a>Příklad JSON: kopírování dat ze služby Salesforce tooAzure objektů Blob
Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak toocopy data ze služby Salesforce tooAzure úložiště objektů Blob. Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.   

Tady jsou hello artefaktů služby Data Factory, budete potřebovat toocreate tooimplement hello scénář. části Hello hello seznamu obsahují podrobnosti o těchto krocích.

* Propojené služby typu hello [Salesforce](#linked-service-properties)
* Propojené služby typu hello [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)
* Vstup [datovou sadu](data-factory-create-datasets.md) typu hello [RelationalTable](#dataset-properties)
* Výstup [datovou sadu](data-factory-create-datasets.md) typu hello [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

**Salesforce propojené služby**

Tento příklad používá hello **Salesforce** propojené služby. V tématu hello [Salesforce propojená služba](#linked-service-properties) části hello vlastnosti, které podporuje tuto propojenou službu.  V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny, jak hello tooreset nebo získat token zabezpečení.

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
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
**Vstupní datové sady služby Salesforce**

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"  
        },
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

Nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

> [!IMPORTANT]
> Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Výstupní datová sada Azure Blob**

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
            "folderPath": "adfgetstarted/alltypes_c"
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

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurována toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource**a hello **podřízený** je typ nastaven příliš**BlobSink**.

V tématu [vlastnosti typu RelationalSource](#copy-activity-properties) hello seznam vlastností, které jsou podporovány hello RelationalSource.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
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
> [!IMPORTANT]
> Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>Mapování typu pro Salesforce
| Typ služby Salesforce | . Na základě NET typu |
| --- | --- |
| Automatické číslování |Řetězec |
| Zaškrtávací políčko |Logická hodnota |
| Měna |Double |
| Datum |Data a času |
| Datum a čas |Data a času |
| E-mail |Řetězec |
| ID |Řetězec |
| Relace hledání |Řetězec |
| Vybrat víc rozevíracího seznamu |Řetězec |
| Číslo |Double |
| Procento |Double |
| Telefon |Řetězec |
| Rozevírací seznam |Řetězec |
| Text |Řetězec |
| Textová oblast |Řetězec |
| Textová oblast (Long) |Řetězec |
| (Rich) textová oblast |Řetězec |
| Text (šifrované) |Řetězec |
| ADRESA URL |Řetězec |

> [!NOTE]
> toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
