---
title: "Kopírování dat ze zdroje ODBC pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak ke zkopírování dat z OData zdroje k úložištím dat. podporované podřízený pomocí aktivity kopírování v kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 14f654979f004186e81b2f18578ced3c9aab3815
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Kopírování dat z a do úložiště dat rozhraní ODBC pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-odbc-connector.md)
> * [Verze 2 – Preview](connector-odbc.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z a k úložišti dat ODBC. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [ODBC konektoru V1](v1/data-factory-odata-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete zkopírovat data ze zdroje ODBC do úložiště dat žádné podporované podřízený nebo zkopírovat z úložiště dat žádné podporované zdrojové do ODBC podřízený. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor ODBC podporuje kopírování dat z/do **úložiště dat žádné kompatibilní se standardem ODBC** pomocí **základní** nebo **anonymní** ověřování.

## <a name="prerequisites"></a>Požadavky

Chcete-li použít tento konektor ODBC, budete muset:

- Nastavte Self-hosted integrace Runtime. V tématu [Self-hosted integrace Runtime](create-self-hosted-integration-runtime.md) článku.
- Nainstalujte ovladač ODBC pro úložiště dat na počítači integrace modulu Runtime.

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory ke konektoru ODBC.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro ODBC propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **rozhraní Odbc** | Ano |
| připojovací řetězec | Připojovací řetězec, s výjimkou části přihlašovací údaje. Můžete zadat připojovací řetězec pomocí vzoru jako `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`, nebo pomocí systému DSN (název zdroje dat), nastavení na počítači Runtime integrace s `"DSN=<name of the DSN on IR machine>;"` (třeba stále zadáte části přihlašovací údaje v propojené službě odpovídajícím způsobem).| Ano |
| authenticationType. | Typ ověřování používaný pro připojení k úložišti dat ODBC.<br/>Povolené hodnoty jsou: **základní** a **anonymní**. | Ano |
| userName | Pokud používáte základní ověřování, zadejte uživatelské jméno. | Ne |
| heslo | Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno. Toto pole můžete označte jako SecureString. | Ne |
| přihlašovací údaje | Část přístup přihlašovacích údajů z připojovacího řetězce zadaného ve formátu ovladačem vlastnost hodnota. Příklad: `"RefreshToken=<secret refresh token>;"`. Toto pole můžete označte jako SecureString. | Ne |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. Modul Runtime Self-hosted integrace se vyžaduje, jak je uvedeno v [požadavky](#prerequisites). |Ano |

**Příklad 1: použití základního ověřování**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Příklad 2: pomocí anonymní ověřování**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností nepodporuje datovou sadu ODBC.

Ke zkopírování dat z/do úložiště dat kompatibilní se standardem ODBC, nastavte vlastnost typu datové sady, která **RelationalTable**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **RelationalTable** | Ano |
| tableName | Název tabulky v úložišti dat ODBC. | Ne pro zdroj (pokud "dotaz" v zdroj aktivity je určena);<br/>Ano pro sink |

**Příklad**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Vlastnosti aktivity kopírování

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje zdroje ODBC.

### <a name="odbc-as-source"></a>ODBC jako zdroj

Ke zkopírování dat z úložiště dat kompatibilní se standardem ODBC, nastavte typ zdroje v aktivitě kopírování do **RelationalSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **RelationalSource** | Ano |
| query | Čtení dat pomocí vlastního dotazu SQL. Například: `"SELECT * FROM MyTable"`. | Ne (když je určena "tableName" v datové sadě) |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="odbc-as-sink"></a>ODBC jako jímku

Ke zkopírování dat do úložiště dat kompatibilní se standardem ODBC, nastavte typ jímky v aktivitě kopírování do **OdbcSink**. Následující vlastnosti jsou podporovány v aktivitě kopírování **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky kopie aktivity musí nastavena: **OdbcSink** | Ano |
| writeBatchTimeout |Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.<br/>Povolené hodnoty jsou: časový interval. Příklad: "00: 30:00" (30 minut). |Ne |
| writeBatchSize |Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.<br/>Povolené hodnoty jsou: celé číslo (počet řádků). |Ne (výchozí hodnota je 0 - zjistil automaticky) |
| preCopyScript |Zadejte pro aktivitu kopírování ke spuštění před zápis dat do úložiště dat v každé spuštění příkazu jazyka SQL. Tato vlastnost slouží k vyčištění předem načtená data. |Ne |

> [!NOTE]
> Pro "writeBatchSize" Pokud není nastaven (automatické rozpoznání), aktivity kopírování nejdřív zjistí, zda ovladač podporuje dávkové operace a nastavte ji na 10000, pokud k tomu nebo nastavený na hodnotu 1, pokud tomu tak není. Pokud nastavíte explicitně hodnotu než 0, aktivity kopírování ctí hodnota a selže za běhu, pokud ovladač nepodporuje dávkových operací.

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="ibm-informix-source"></a>Zdroj IBM Informix

Můžete zkopírovat data z databáze IBM Informix pomocí obecné konektoru ODBC.

Nastavení integrace Runtime Self-hosted na počítači s přístupem do vašeho úložiště. Modul Runtime integrace používá ovladač ODBC pro Informix pro připojení k úložišti. Nainstalujte ovladač, proto, pokud ještě není nainstalovaná na stejném počítači. Například můžete použít ovladač "Ovladač ODBC IBM INFORMIX (64 bitů)". V tématu [požadavky](#prerequisites) podrobnosti.

Než použijete zdroji Informix v řešení pro vytváření dat, ověřte, zda modul Runtime integrace se mohou připojit k úložišti dat pomocí pokynů v [problémů s připojením](#troubleshoot-connectivity-issues) části.

Vytvoření služby ODBC propojené úložiště dat IBM Informix propojení s objektem pro vytváření dat Azure, jak je znázorněno v následujícím příkladu:

```json
{
    "name": "InformixLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<Informix connection string or DSN>"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Čtení článku od začátku podrobnější přehled použití dat ODBC ukládá jako zdroj/jímka datová úložiště v operaci kopírování.

## <a name="microsoft-access-source"></a>Zdroj Microsoft Access

Můžete zkopírovat data z databáze aplikace Microsoft Access pomocí obecné konektoru ODBC.

Nastavení integrace Runtime Self-hosted na počítači s přístupem do vašeho úložiště. Modul Runtime integrace používá ovladač ODBC Microsoft Access k připojení k úložišti. Nainstalujte ovladač, proto, pokud ještě není nainstalovaná na stejném počítači. V tématu [požadavky](#prerequisites) podrobnosti.

Než použijete zdroji Microsoft Access v řešení pro vytváření dat, ověřte, zda modul Runtime integrace se mohou připojit k úložišti dat pomocí pokynů v [problémů s připojením](#troubleshoot-connectivity-issues) části.

Vytvoření služby ODBC propojené databázi aplikace Microsoft Access propojení s objektem pro vytváření dat Azure, jak je znázorněno v následujícím příkladu:

```json
{
    "name": "MicrosoftAccessLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={Microsoft Access Driver (*.mdb, *.accdb)};Dbq=<path to your DB file e.g. C:\\mydatabase.accdb>;"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Čtení článku od začátku podrobnější přehled použití dat ODBC ukládá jako zdroj/jímka datová úložiště v operaci kopírování.

## <a name="ge-historian-source"></a>GE Historian zdroje

Data můžete zkopírovat z GE Historian pomocí obecné konektoru ODBC.

Nastavení integrace Runtime Self-hosted na počítači s přístupem do vašeho úložiště. Modul Runtime integrace používá ovladač ODBC pro GE Historian pro připojení k úložišti. Nainstalujte ovladač, proto, pokud ještě není nainstalovaná na stejném počítači. V tématu [požadavky](#prerequisites) podrobnosti.

Než použijete zdroji GE Historian v řešení pro vytváření dat, ověřte, zda modul Runtime integrace se mohou připojit k úložišti dat pomocí pokynů v [problémů s připojením](#troubleshoot-connectivity-issues) části.

Vytvoření služby ODBC propojené databázi aplikace Microsoft Access propojení s objektem pro vytváření dat Azure, jak je znázorněno v následujícím příkladu:

```json
{
    "name": "HistorianLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<GE Historian store connection string or DSN>"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Čtení článku od začátku podrobnější přehled použití dat ODBC ukládá jako zdroj/jímka datová úložiště v operaci kopírování.

## <a name="sap-hana-sink"></a>Podřízený SAP HANA

>[!NOTE]
>Kopírování dat z úložiště dat SAP HANA, najdete v tématu nativní [konektor SAP HANA](connector-sap-hana.md). Ke zkopírování dat do SAP HANA, postupujte podle tento pokyn k používání konektoru ODBC. Všimněte si, které jsou propojené služby pro SAP HANA connector a konektor ODBC s jiným typem proto nelze znovu použít.
>

Zkopírovat data do databáze SAP HANA pomocí obecné konektoru ODBC.

Nastavení integrace Runtime Self-hosted na počítači s přístupem do vašeho úložiště. Modul Runtime integrace používá ovladač ODBC pro SAP HANA pro připojení k úložišti. Nainstalujte ovladač, proto, pokud ještě není nainstalovaná na stejném počítači. V tématu [požadavky](#prerequisites) podrobnosti.

Než použijete jímky SAP HANA v řešení pro vytváření dat, ověřte, zda modul Runtime integrace se mohou připojit k úložišti dat pomocí pokynů v [problémů s připojením](#troubleshoot-connectivity-issues) části.

Vytvoření služby ODBC propojené úložiště dat SAP HANA propojení s objektem pro vytváření dat Azure, jak je znázorněno v následujícím příkladu:

```json
{
    "name": "SAPHANAViaODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={HDBODBC};servernode=<HANA server>.clouddatahub-int.net:30015"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Čtení článku od začátku podrobnější přehled použití dat ODBC ukládá jako zdroj/jímka datová úložiště v operaci kopírování.

## <a name="troubleshoot-connectivity-issues"></a>Řešení potíží s problémy s připojením

K odstraňování problémů s připojením, použijte **diagnostiky** kartě **integrace modulu Runtime Configuration Manager**.

1. Spusťte **integrace modulu Runtime Configuration Manager**.
2. Přepnout **diagnostiky** kartě.
3. V části "Test připojení", vyberte **typ** dat uložit (propojené služby).
4. Zadejte **připojovací řetězec** sloužící k připojení k úložišti dat, vyberte **ověřování** a zadejte **uživatelské jméno**, **heslo**, nebo **pověření**.
5. Klikněte na tlačítko **testovací připojení** k testování připojení k úložišti.

## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).