---
title: "aaaMove data z MongoDB pomocí služby Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z MongoDB databáze pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 154e85712f27b978976c7499c43dde9429f124c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Přesun dat z MongoDB pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove datech z místní databáze MongoDB. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní MongoDB data. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat tooother uložit data z datové MongoDB, ale ne pro přesun dat z jiných dat úložiště tooan MongoDB úložiště. 

## <a name="prerequisites"></a>Požadavky
Pro hello Azure Data Factory služby toobe možné tooconnect tooyour místní databázi MongoDB je třeba nainstalovat hello následující součásti:

- Podporované verze MongoDB jsou: 2.4, 2.6, 3.0 a 3.2.
- Brána pro správu dat v databázi hello hostitelů hello stejný počítač nebo na samostatný počítač tooavoid, neslučitelných pro prostředky s databází hello. Brána pro správu dat je software, který se připojuje místní datové zdroje toocloud služby způsobem, zabezpečení a správě. V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku. V tématu [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení brány hello toomove data datového kanálu.

    Při instalaci brány hello se automaticky nainstaluje tooMongoDB tooconnect použít ovladače Microsoft MongoDB ODBC.

    > [!NOTE]
    > Je nutné toouse hello brány tooconnect tooMongoDB i v případě, že je hostován v virtuální počítače Azure IaaS. Pokud se pokoušíte tooconnect tooan instanci MongoDB hostované v cloudu, můžete také nainstalovat instanci brány hello v hello virtuálních počítačů IaaS.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní MongoDB data pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru **, **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello: 

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z místního úložiště dat MongoDB, naleznete v tématu [JSON příklad: kopírování dat z MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooMongoDB zdroje:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis pro konkrétní elementy JSON příliš**OnPremisesMongoDB** propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **OnPremisesMongoDb** |Ano |
| server |IP adresa nebo název hostitele serveru MongoDB hello. |Ano |
| port |Port TCP, který hello serveru MongoDB toolisten používá pro připojení klientů. |Volitelné, výchozí hodnota: 27017 |
| authenticationType. |Základní, nebo anonymní. |Ano |
| uživatelské jméno |Uživatel účet tooaccess MongoDB. |Ano (Pokud se používá základní ověřování). |
| heslo |Heslo pro uživatele hello. |Ano (Pokud se používá základní ověřování). |
| authSource |Název databáze hello MongoDB, že chcete toouse toocheck přihlašovacích údajů pro ověřování. |Volitelný parametr (Pokud se používá základní ověřování). Výchozí: používá účet správce hello a hello databáze zadat pomocí vlastnost databaseName. |
| Název databáze |Název databáze hello MongoDB, které chcete tooaccess. |Ano |
| gatewayName |Název brány hello, který přistupuje k úložišti dat hello. |Ano |
| encryptedCredential |Přihlašovací údaje zašifrovaná pomocí brány. |Nepovinné |

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **MongoDbCollection** má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Název_kolekce |Název kolekce hello v databázi MongoDB. |Ano |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivit na hello se každý typ aktivity lišit podle druhé straně. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pokud zdroj hello je typu **MongoDbSource** hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL 92. Příklad: vybrat * z MyTable. |Ne (Pokud **Název_kolekce** z **datovou sadu** je zadána) |



## <a name="json-example-copy-data-from-mongodb-tooazure-blob"></a>Příklad JSON: kopírování dat z MongoDB tooAzure objektů Blob
Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazuje, jak toocopy data ze tooan MongoDB místní úložiště objektů Blob Azure. Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

Ukázka Hello má hello následující entity objektu pro vytváření dat:

1. Propojené služby typu [OnPremisesMongoDb](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [MongoDbCollection](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [MongoDbSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databázi MongoDB každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Jako první krok, instalační program hello Brána pro správu dat podle pokynů hello v hello [Brána pro správu dat](data-factory-data-management-gateway.md) článku.

**MongoDB propojené služby:**

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",  
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
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

**Vstupní datové sady MongoDB:** nastavení "externí": "PRAVDA" informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
     "name":  "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"    
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob výstupní datovou sadu:**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Aktivita kopírování v kanálu s MongoDB zdrojový a podřízený objekt Blob:**

Hello kanál obsahuje aktivitu kopírování, nakonfigurované toouse hello výše vstupní a výstupní datové sady, který je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**MongoDbSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
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
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a>Schéma službou Data Factory
Služba Azure Data Factory odvodí schématu z kolekce MongoDB pomocí hello nejnovější 100 dokumenty v kolekci hello. Pokud tyto dokumenty 100 neobsahují úplné schéma, může být během operace kopírování hello ignorován některé sloupce.

## <a name="type-mapping-for-mongodb"></a>Mapování typu pro MongoDB
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesunu dat tooMongoDB hello následující mapování se používají z typů too.NET typy MongoDB.

| Typ MongoDB | Typ rozhraní .NET framework |
| --- | --- |
| Binární |Byte] |
| Logická hodnota |Logická hodnota |
| Datum |Data a času |
| NumberDouble |Double |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |Řetězec |
| Řetězec |Řetězec |
| UUID |Identifikátor GUID |
| Objekt |Renormalized do vyrovnání sloupce s "_" jako vnořené oddělovače |

> [!NOTE]
> toolearn o podpoře pro polí pomocí virtuální tabulky odkazovat příliš[podporu pro komplexní typy pomocí virtuální tabulky](#support-for-complex-types-using-virtual-tables) části níže.

V současné době nejsou podporovány následující typy dat MongoDB hello: DBPointer, JavaScript, Max za minutu klíče, regulární výraz, Symbol, časové razítko, Undefined

## <a name="support-for-complex-types-using-virtual-tables"></a>Podpora pro komplexní typy pomocí virtuální tabulky
Azure Data Factory používá integrované ODBC ovladač tooconnect tooand kopírování dat z databáze MongoDB. Pro komplexní typy jako pole nebo objekty s různými typy na dokumentech hello hello ovladač znovu sjednotí data na odpovídající virtuální tabulky. Konkrétně Pokud tabulka obsahuje tyto sloupce, hello ovladač generuje hello následující virtuální tabulky:

* A **základní tabulka**, který obsahuje hello stejná data jako hello skutečné tabulky s výjimkou hello komplexní typ sloupce. Základní tabulka Hello používá hello stejný název jako hello skutečné tabulku, která reprezentuje.
* A **virtuální tabulku** pro každý sloupec komplexní typ, který rozbalí hello vnořená data. virtuální tabulky Hello jsou pojmenované pomocí hello název hello skutečné tabulky, oddělovače "_" a název hello hello pole nebo objekt.

Virtuální tabulky odkazovat toohello data v tabulce skutečné hello, povolení tooaccess ovladač hello hello nenormalizované data. Viz část o příklad níže podrobnosti. Obsah hello MongoDB polí můžete přejít pomocí dotazování a spojování tabulek virtuálním hello.

Můžete použít hello [Průvodce kopírováním](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively zobrazení hello seznam tabulek v databázi MongoDB, včetně hello virtuální tabulky a náhled obsažená data hello. Můžete také vytvořit dotaz v hello Průvodce kopírováním a ověřit toosee hello výsledek.

### <a name="example"></a>Příklad
Například "ExampleTable" pod je MongoDB tabulku, která obsahuje jeden sloupec s pole objektů v každé buňce – faktury a jeden sloupec s pole Skalární typy – hodnocení.

| _id | Jméno zákazníka | Faktury | Úrovně služeb | Hodnocení |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123", položka: "Toaster byl", cena: "456", slevu: "0,2"}, {invoice_id: "124", položka: "sušárny", ceny: slevách "1235": "0,2"}] |Stříbrná |[5,6] |
| 2222 |XYZ |[{invoice_id: "135", položka: "ledničky", cena: "12543", slevu: "0,0"}] |Zlatý |[1,2] |

ovladač Hello by vygeneroval více toorepresent virtuální tabulky tento jednu tabulku. první virtuální tabulky Hello je hello základní tabulka s názvem "ExampleTable", viz následující obrázek. Hello základní tabulka obsahuje všechna data hello hello původní tabulky, ale hello data z pole hello byla vynechána a je rozšířit hello virtuální tabulky.

| _id | Jméno zákazníka | Úrovně služeb |
| --- | --- | --- |
| 1111 |ABC |Stříbrná |
| 2222 |XYZ |Zlatý |

Hello následující tabulky popisují hello virtuální tabulky, které představují hello původní pole v příkladu hello. Tyto tabulky obsahují hello následující:

* Odkaz zpět toohello původní sloupec primárního klíče odpovídající toohello řádek hello původní pole (přes sloupec _id hello)
* Označení pozice hello hello dat v rámci pole původní hello
* Hello rozšířit dat pro každý prvek v rámci pole hello

Tabulka "ExampleTable_Invoices":

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | Položka | price | Sleva |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |Toaster byl |456 |0.2 |
| 1111 |1 |124 |sušárny |1235 |0.2 |
| 2222 |0 |135 |ledničky |12543 |0.0 |

Tabulka "ExampleTable_Ratings":

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.

## <a name="next-steps"></a>Další kroky
V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článek podrobné pokyny pro vytváření dat kanál, který přesouvá data z místních dat úložiště Azure data tooan.
