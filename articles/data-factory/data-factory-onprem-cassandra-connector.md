---
title: "aaaMove data z Cassandra pomocí služby Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z Cassandra místní databázi pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 0e265d3a8439d0a2cb2a5c32e5ea8348a1617621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Přesun dat z databáze Cassandra místně pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze Cassandra místně. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní Cassandra data. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat z datové Cassandra tooother datová úložiště, ale ne pro přesun dat z jiného úložiště dat úložiště tooa Cassandra data. 

## <a name="supported-versions"></a>Podporované verze
Hello Cassandra konektor podporuje následující verze Cassandra hello: 2.X.

## <a name="prerequisites"></a>Požadavky
Hello Azure Data Factory toobe možné tooconnect tooyour místní Cassandra databáze služby, je nutné nainstalovat brána pro správu dat na hello stejný počítač databázi hello hostitele nebo na samostatný počítač tooavoid, neslučitelných pro prostředky s hello databáze. Brána pro správu dat je komponenta, která připojuje místní datové zdroje toocloud služby v zabezpečený a spravovaný. V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku. V tématu [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení brány hello toomove data datového kanálu.

Hello brány tooconnect tooa Cassandra databázi je nutné použít i v případě, že hello je databáze hostována v cloudu hello, například na virtuálním počítači Azure IaaS. Y, které máte hello brány na hello databázi hello hostitelů stejného virtuálního počítače nebo na samostatný virtuální počítač stejně dlouho jako hello brány můžete připojit toohello databáze.  

Při instalaci brány hello se automaticky nainstaluje Microsoft Cassandra ODBC ovladače použít tooconnect tooCassandra databáze. Proto není nutné toomanually všechny ovladače nainstalovat na počítač brány hello při kopírování dat z databáze Cassandra hello. 

> [!NOTE]
> V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API. 

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello. 
- Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat Cassandra místní, naleznete v tématu [JSON příklad: kopírování dat z Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa Cassandra úložiště dat:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooCassandra propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **OnPremisesCassandra** |Ano |
| hostitele |Jeden nebo více IP adres nebo názvů hostitelů Cassandra serverů.<br/><br/>Zadejte seznam IP adres nebo serverům tooall tooconnect názvy hostitele současně. |Ano |
| port |port TCP, který hello Cassandra server Hello používá toolisten pro připojení klientů. |Ne, výchozí hodnota: 9042 |
| authenticationType. |Basic nebo Anonymous |Ano |
| uživatelské jméno |Zadejte uživatelské jméno pro hello uživatelský účet. |Ano, pokud je nastavená authenticationType tooBasic. |
| heslo |Zadejte heslo pro uživatelský účet hello. |Ano, pokud je nastavená authenticationType tooBasic. |
| gatewayName |Název Hello hello bránu, která je použité tooconnect toohello místní Cassandra databázi. |Ano |
| encryptedCredential |Přihlašovací údaje šifrované bránou hello. |Ne |

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **CassandraTable** má následující vlastnosti hello

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| keyspace |Název schématu v databázi Cassandra nebo hello keyspace. |Ano (Pokud **dotazu** pro **CassandraSource** není definován). |
| tableName |Název hello tabulky v databázi Cassandra. |Ano (Pokud **dotazu** pro **CassandraSource** není definován). |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pokud je zdroj typu **CassandraSource**, hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Dotaz SQL 92 nebo CQL dotazu. V tématu [CQL odkaz](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Při použití příkazu jazyka SQL, zadejte **keyspace name.table název** toorepresent hello tabulky, které mají být tooquery. |Ne (pokud jsou definovány tableName a keyspace v sadě dat). |
| consistencyLevel |úroveň konzistence Hello Určuje, kolik repliky musí odpovídat požadavků na čtení tooa před vrácením dat toohello klientskou aplikaci. Kontroly Cassandra hello zadaný počet replik pro data toosatisfy hello čtení požadavku. |JEDEN, DVA, TŘI, KVORA, VŠE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. V tématu [konfigurace konzistenci dat](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) podrobnosti. |Ne. Výchozí hodnota je 1. |

## <a name="json-example-copy-data-from-cassandra-tooazure-blob"></a>Příklad JSON: kopírování dat z Cassandra tooAzure objektů Blob
Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazuje, jak toocopy data z Cassandra místní databázi tooan Azure Blob Storage. Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

> [!IMPORTANT]
> Tato ukázka obsahuje fragmenty kódu JSON. Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.

Ukázka Hello má hello následující entity objektu pro vytváření dat:

* Propojené služby typu [OnPremisesCassandra](#linked-service-properties).
* Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Vstup [datovou sadu](data-factory-create-datasets.md) typu [CassandraTable](#dataset-properties).
* Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [CassandraSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

**Cassandra propojené služby:**

Tento příklad používá hello **Cassandra** propojené služby. V tématu [Cassandra propojená služba](#linked-service-properties) části vlastností hello nepodporuje tuto propojenou službu.  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Propojená služba Azure Storage:**

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

**Cassandra vstupní datové sady:**

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
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

**Azure Blob výstupní datovou sadu:**

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
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Aktivita kopírování v kanálu s Cassandra zdrojový a podřízený objekt Blob:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**CassandraSource** a **podřízený** je typ nastaven příliš**BlobSink**.

V tématu [vlastnosti typu RelationalSource](#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem hello RelationalSource.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

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

### <a name="type-mapping-for-cassandra"></a>Mapování typu pro Cassandra
| Typ Cassandra | .NET na základě typu |
| --- | --- |
| ASCII |Řetězec |
| BIGINT |Int64 |
| OBJEKT BLOB |Byte] |
| LOGICKÁ HODNOTA |Logická hodnota |
| DECIMAL |Decimal |
| DOUBLE |Double |
| PLOVOUCÍ DESETINNÁ ČÁRKA |Jeden |
| INET |Řetězec |
| INT |Int32 |
| TEXT |Řetězec |
| ČASOVÉ RAZÍTKO |Data a času |
| TIMEUUID |Identifikátor GUID |
| UUID |Identifikátor GUID |
| VARCHAR |Řetězec |
| VARINT |Decimal |

> [!NOTE]
> Kolekce typů (mapy, sady, seznamu atd.), najdete v části příliš[pracovat s typy kolekcí Cassandra pomocí virtuální tabulky](#work-with-collections-using-virtual-table) části.
>
> Uživatelem definované typy nejsou podporovány.
>
> Hello délka binárního sloupce a sloupce, řetězec délky nemůže být větší než 4000.
>
>

## <a name="work-with-collections-using-virtual-table"></a>Práce s kolekcí pomocí virtuální tabulky
Azure Data Factory používá integrované ODBC ovladač tooconnect tooand kopírování dat z databáze Cassandra. Pro typy kolekcí včetně mapy, sady a seznamem hello ovladač renormalizes hello data do odpovídající virtuální tabulky. Konkrétně Pokud tabulka obsahuje všechny sloupce, kolekce, hello ovladač generuje hello následující virtuální tabulky:

* A **základní tabulka**, který obsahuje hello stejná data jako hello skutečné tabulky s výjimkou hello kolekce sloupců. Základní tabulka Hello používá hello stejný název jako hello skutečné tabulku, která reprezentuje.
* A **virtuální tabulku** pro každý sloupec kolekce, které rozšíří hello vnořená data. Hello virtuální tabulky, které představují kolekce jsou pojmenované pomocí názvu hello hello skutečné tabulky, oddělovače "*vt*" a hello název sloupce hello.

Virtuální tabulky odkazovat toohello data v tabulce skutečné hello, povolení tooaccess ovladač hello hello nenormalizované data. Příklad naleznete v části Podrobnosti. Obsah hello Cassandra kolekcí můžete přejít pomocí dotazování a spojování tabulek virtuálním hello.

Můžete použít hello [Průvodce kopírováním](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively zobrazení hello seznam tabulek v databázi Cassandra včetně hello virtuální tabulky a zobrazení náhledu obsažená data hello. Můžete také vytvořit dotaz v hello Průvodce kopírováním a ověřit toosee hello výsledek.

### <a name="example"></a>Příklad
Například hello následující "ExampleTable" je Cassandra tabulku databáze, která obsahuje celé číslo primární klíče sloupec s názvem "pk_int", o textový sloupec s názvem hodnotu, sloupce seznamu, mapy sloupec a sadu sloupec (s názvem "StringSet").

| pk_int | Hodnota | Seznam | mapy | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"Ukázková hodnota 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"Ukázková hodnota 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

ovladač Hello by vygeneroval více toorepresent virtuální tabulky tento jednu tabulku. Hello sloupce cizích klíčů v tabulkách virtuálním hello odkazovat hello primární klíčových sloupců v tabulce skutečné hello a indikují které reálné odpovídá řádku virtuální tabulku hello řádek tabulky.

první virtuální tabulky Hello je hello základní tabulka s názvem "ExampleTable" se zobrazí v hello následující tabulka. Hello základní tabulka obsahuje hello stejná data jako hello původní tabulky databáze s výjimkou hello kolekce, které jsou vynechání z této tabulky a rozšířit ostatní virtuální tabulky.

| pk_int | Hodnota |
| --- | --- |
| 1 |"Ukázková hodnota 1" |
| 3 |"Ukázková hodnota 3" |

Hello následující tabulky popisují hello virtuální tabulky, které renormalize hello data ze seznamu a mapy, StringSet sloupce hello. Hello sloupce s názvy, které končí "_index" nebo "_key" označuje pozici hello hello dat v rámci původní seznam hello nebo mapy. Hello sloupce s názvy, které končí "_value" obsahují hello rozšířit data z kolekce hello.

#### <a name="table-exampletablevtlist"></a>Tabulka "ExampleTable_vt_List":
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-exampletablevtmap"></a>Tabulka "ExampleTable_vt_Map":
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |B |
| 3 |S1 |T |

#### <a name="table-exampletablevtstringset"></a>Tabulka "ExampleTable_vt_StringSet":
| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
