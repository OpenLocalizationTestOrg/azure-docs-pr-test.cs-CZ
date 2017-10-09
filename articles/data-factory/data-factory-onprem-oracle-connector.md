---
title: "aaaCopy data do nebo z databáze Oracle pomocí služby Data Factory | Microsoft Docs"
description: "Zjistěte, jak toocopy data z databáze Oracle, který je místně pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: adb6d5fbe38e18791616ac77e8179970bbea37fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Kopírování dat z místní Oracle pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze Oracle na místě. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

## <a name="supported-scenarios"></a>Podporované scénáře
Může kopírovat data **z databáze Oracle** toohello následující úložišť dat:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Data můžete zkopírovat z hello následující úložišť dat **databáze Oracle tooan**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Požadavky
Objekt pro vytváření dat podporuje připojování zdroje Oracle tooon místní pomocí hello Brána pro správu dat. V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) toolearn článek o Brána pro správu dat a [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení brány hello datovém kanálu toomove data.

Vyžaduje se brána, i když hello Oracle je hostovaná ve virtuálním počítači Azure IaaS. Hello brány můžete nainstalovat na stejný virtuální počítač IaaS jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit databázi toohello hello.

> [!NOTE]
> V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.

## <a name="supported-versions-and-installation"></a>Podporované verze a instalaci
Tento konektor Oracle podporují dvě verze ovladače:

- **Ovladač Microsoft pro Oracle (doporučeno)**: spouštění z brány správy dat verze 2.7 ovladač Microsoft pro Oracle se nainstaluje automaticky společně hello brány, proto není nutné tooadditionally popisovač hello ovladače v pořadí tooOracle tooestablish připojení a může také nastat, lepší výkon kopírování pomocí tohoto ovladače. Následující verze systému Oracle jsou podporovány databází:
    - R1 Oracle 12c (12.1)
    - R1 Oracle 11g nebo R2 (11.1, 11.2)
    - R1 Oracle 10g, R2 (10,1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

> [!IMPORTANT]
> Ovladač Microsoft pro Oracle aktuálně podporuje jenom kopírování dat z Oracle, ale není zápis tooOracle. A Všimněte si, že hello test připojení funkci karta diagnostiku brány správy dat nepodporuje tento ovladač. Alternativně můžete použít hello kopie Průvodce toovalidate hello připojení.
>

- **Zprostředkovatel dat Oracle pro .NET:** můžete také toouse poskytovatele dat Oracle toocopy data z / tooOracle. Tato součást je součástí [Oracle Data přístup součásti pro Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Nainstalujte příslušnou verzi hello (32 nebo 64bitový) hello počítače, kde je nainstalován hello brány. [Zprostředkovatel dat Oracle .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) přístup tooOracle Database 10 g verze 2 nebo novější.

    Pokud si zvolíte možnost "XCopy instalace", postupujte podle kroků v hello readme.htm. Doporučujeme že vybrat instalační program hello pomocí uživatelského rozhraní (bez XCopy jeden).

    Po instalaci poskytovatele hello **restartujte** hello hostitelská služba Brána pro správu dat na počítači pomocí služby aplet (nebo) Správce konfigurace brány pro správu dat.  

Pokud používáte kopie Průvodce tooauthor hello kopie kanálu, typu ovladač hello bude určen automaticky. Ovladač Microsoft použije ve výchozím nastavení, pokud vaše verze brány je nižší než 2.7 nebo zvolte Oracle jako jímku.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z databáze Oracle místně pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **objekt pro vytváření dat**. Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů. 
2. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory. Například pokud kopírování dat z databáze tooan Oralce úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink databáze Oracle a účet úložiště Azure tooyour služby data factory. Vlastnosti propojené služby, které jsou specifické tooOracle, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.
3. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. V příkladu hello uvedených v posledním kroku hello vytvoříte tabulku hello toospecify datové sady ve vaší databázi Oracle, který obsahuje vstupní data hello. Vytvoření kontejneru objektů blob hello toospecify s jinou datovou sadu a hello složky, která obsahuje hello data zkopírovaných z hello databáze Oracle. Vlastnosti datové sady, které jsou specifické tooOracle, najdete v části [vlastnosti datové sady](#dataset-properties) části.
4. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. V příkladu hello již bylo zmíněno dříve použijete OracleSource jako zdroj a BlobSink jako jímku pro aktivitu kopírování hello. Podobně pokud zkopírujete z Azure Blob Storage tooOracle databáze, použijte BlobSource a OracleSink v aktivitě kopírování hello. Vlastnosti aktivity kopírování, které jsou specifické tooOracle databáze, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části. Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo z databáze Oracle na místě, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-oracle-database) tohoto článku.

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine entit služby Data Factory:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooOracle propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **OnPremisesOracle** |Ano |
| driverType | Určete, jaká data toocopy toouse ovladače z / tooOracle databáze. Povolené hodnoty jsou **Microsoft** nebo **ODP** (výchozí). V tématu [podporované verze a instalace](#supported-versions-and-installation) části na podrobnosti o ovladači. | Ne |
| připojovací řetězec | Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello databáze Oracle instance. | Ano |
| gatewayName | Název hello brány, který je použité tooconnect toohello místního serveru Oracle |Ano |

**Příklad: pomocí ovladače Microsoft:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Příklad: pomocí ODP ovladače**

Odkazovat příliš[tento web](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) pro hello povolené formáty.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Oracle, objektů blob v Azure, Azure table atd.).

část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. Hello rámci typeProperties části pro datovou sadu hello typu OracleTable má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v hello databáze Oracle, který hello propojená služba odkazuje. |Ne (Pokud **oracleReaderQuery** z **OracleSource** je zadána) |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

> [!NOTE]
> Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.

Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

### <a name="oraclesource"></a>OracleSource
Při aktivitě kopírování, pokud je zdroj hello typu **OracleSource** hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| oracleReaderQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Příklad: vybrat * z MyTable <br/><br/>Pokud není zadaný, hello příkaz jazyka SQL, která se provedla: vybrat * z MyTable |Ne (Pokud **tableName** z **datovou sadu** je zadána) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** podporuje hello následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| writeBatchTimeout |Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: 00:30:00 (30 minut). |Ne |
| writeBatchSize |Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello. |Celé číslo (počet řádků) |Ne (výchozí: 100) |
| sqlWriterCleanupScript |Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez. |Příkaz dotazu. |Ne |
| sliceIdentifierColumnName |Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit. |Název sloupce sloupce s datovým typem binary(32). |Ne |

## <a name="json-examples-for-copying-data-tooand-from-oracle-database"></a>Příklady JSON pro kopírování data tooand z databáze Oracle
Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak toocopy data z / tooan Oracle databáze do/z Azure Blob Storage. Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.   

## <a name="example-copy-data-from-oracle-tooazure-blob"></a>Příklad: Kopírování dat z Oracle tooAzure objektů Blob

Ukázka Hello má hello následující entity objektu pro vytváření dat:

1. Propojené služby typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) jako zdroj a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) jako jímku.

Ukázka Hello zkopíruje data z tabulky v objektu blob tooa databáze Oracle místní každou hodinu. Další informace o různých vlastnosti používané v ukázce hello najdete v dokumentaci k v částech následující ukázky hello.

**Oracle propojené služby:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Propojená služba Azure Blob storage:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Oracle vstupní datové sady:**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Oracle a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.

Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
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

**Azure Blob výstupní datovou sadu:**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kanál s aktivitou kopírování:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**OracleSource** a **podřízený** je typ nastaven příliš**BlobSink**.  Zadaný dotaz SQL Hello **oracleReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## <a name="example-copy-data-from-azure-blob-toooracle"></a>Příklad: Kopírování dat z Azure Blob tooOracle
Tento příklad ukazuje, jak toocopy data ze Azure Blob Storage tooan místní databáze Oracle. Nicméně je možné zkopírovat data **přímo** ze všech zdrojů hello uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.  

Ukázka Hello má hello následující entity objektu pro vytváření dat:

1. Propojené služby typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) jako zdroj [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) jako jímku.

Ukázka Hello zkopíruje data z objektu blob tooa tabulky v databázi Oracle místní každou hodinu. Další informace o různých vlastnosti používané v ukázce hello najdete v dokumentaci k v částech následující ukázky hello.

**Oracle propojené služby:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Propojená služba Azure Blob storage:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Azure vstupní datovou sadu objektu Blob**

Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas. "externí": "PRAVDA" nastavení informuje hello služba Data Factory, tato tabulka je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
            "frequency": "Day",
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

**Oracle výstupní datovou sadu:**

Ukázka Hello předpokládá, že jste vytvořili tabulku "MyTable" v Oracle. Vytvoření tabulky hello v Oracle s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello. Přidávání řádků tabulky toohello každou hodinu.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Kanál s aktivitou kopírování:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a hello **podřízený** je typ nastaven příliš**OracleSink**.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a>Rady pro řešení potíží
### <a name="problem-1-net-framework-data-provider"></a>Problém 1: Zprostředkovatel dat .NET Framework

Zobrazí následující hello **chybová zpráva**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable toofind hello requested .Net Framework Data Provider. It may not be installed”.  

**Možné příčiny:**

1. Zprostředkovatel dat .NET Framework pro Oracle Hello nebyl nainstalován.
2. Hello zprostředkovatel dat .NET Framework pro Oracle byla nainstalovaná too.NET Framework 2.0 a nebyla nalezena ve složkách hello rozhraní .NET Framework 4.0.

**Řešení nebo alternativní řešení:**

1. Pokud jste nenainstalovali hello poskytovatele .NET pro Oracle, [ji nainstalovat](http://www.oracle.com/technetwork/topics/dotnet/downloads/) a opakujte hello scénář.
2. Pokud se zobrazí chybová zpráva hello i po instalaci poskytovatele hello, hello následující kroky:
   1. Konfigurace počítače rozhraní .NET 2.0 otevřít ze složky hello: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Vyhledejte **poskytovatele dat Oracle pro .NET**, a mělo by být možné toofind položku, jak je znázorněno v následující ukázka v části hello **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Poskytovatele dat oracle pro .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Kopírování souboru machine.config. Tato položka toohello v následující složce v4.0 hello: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config a too4.xxx.x.x verze změnu hello.
4. Instalace "< cesta instalace ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" do hello globální mezipaměti sestavení (GAC) spuštěním `gacutil /i [provider path]`. ## Tipy pro odstraňování potíží

### <a name="problem-2-datetime-formatting"></a>Problém 2: formátování data a času

Zobrazí následující hello **chybová zpráva**:

    Message=Operation failed in Oracle Database with hello following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Řešení nebo alternativní řešení:**

Řetězec dotazu hello tooadjust může být nutné v aktivitě kopírování založené na tom, jak jsou nakonfigurované data do databáze Oracle, jak je znázorněno v následující hello ukázkové (pomocí funkce to_date hello):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Mapování typu pro Oracle
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesouvání dat z databáze Oracle, se používají hello následující mapování z typu too.NET typ dat Oracle a naopak.

| Oracle datový typ | Datový typ rozhraní .NET framework |
| --- | --- |
| BFILE |Byte] |
| OBJEKT BLOB |Byte] |
| CHAR – |Řetězec |
| DATOVÝ TYP CLOB |Řetězec |
| DATUM |Data a času |
| PLOVOUCÍ DESETINNÁ ČÁRKA |Decimal, řetězec (Pokud přesnost > 28) |
| CELÉ ČÍSLO |Decimal, řetězec (Pokud přesnost > 28) |
| INTERVAL tooMONTH roku |Int32 |
| INTERVAL den tooSECOND |Časový interval |
| DLOUHÁ |Řetězec |
| DLOUHO NEZPRACOVANÁ |Byte] |
| NCHAR |Řetězec |
| NCLOB |Řetězec |
| ČÍSLO |Decimal, řetězec (Pokud přesnost > 28) |
| NVARCHAR2 |Řetězec |
| NEZPRACOVANÁ |Byte] |
| ID ŘÁDKU |Řetězec |
| ČASOVÉ RAZÍTKO |Data a času |
| ČASOVÉ RAZÍTKO S MÍSTNÍM ČASOVÉM PÁSMU |Data a času |
| ČASOVÉ RAZÍTKO S ČASOVÝM PÁSMEM |Data a času |
| CELÉ ČÍSLO BEZ ZNAMÉNKA |Číslo |
| VARCHAR2 |Řetězec |
| XML |Řetězec |

> [!NOTE]
> Datový typ **INTERVAL roku tooMONTH** a **INTERVAL den tooSECOND** nejsou podporována při použití ovladače Microsoft.

## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
