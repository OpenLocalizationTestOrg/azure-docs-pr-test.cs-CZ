---
title: "aaaMove data ze serveru SAP Business Warehouse pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data ze serveru SAP Business Warehouse pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 85df16f4759a846f578cad301e3cf918179143d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Přesun dat z SAP Business Warehouse pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z místní SAP Business Warehouse (BW). Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní SAP Business Warehouse data. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat v současné době podporuje pouze přesouvání dat od tooother SAP Business Warehouse ukládá, ale ne pro přesun dat z jiných dat ukládá tooan SAP Business Warehouse. 

## <a name="supported-versions-and-installation"></a>Podporované verze a instalaci
Tento konektor podporuje verze SAP Business Warehouse 7.x. Podporuje kopírování dat z InfoCubes a QueryCubes (včetně BEx dotazy) pomocí dotazů MDX.

tooenable hello připojení toohello instance SAP BW nainstalovat hello následující součásti:
- **Brána pro správu dat**: podporuje služby Data Factory připojení tooon místní datové úložiště (včetně SAP Business Warehouse) pomocí součásti názvem Brána pro správu dat. v tématu toolearn o Brána pro správu dat a podrobné pokyny pro nastavení brány hello [přesouvání dat mezi místní data úložiště dat toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku. Vyžaduje se brána, i když hello SAP Business Warehouse je hostovaná ve virtuálním počítači Azure IaaS (VM). Hello brány můžete nainstalovat na hello stejného virtuálního počítače jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit toohello databáze.
- **Knihovna SAP NetWeaver** na počítači brány hello. Knihovna SAP Netweaver hello můžete získat od správce SAP, nebo přímo z hello [SAP služby Stažení softwaru](https://support.sap.com/swdc). Vyhledejte hello **&#1025361; Poznámka SAP** umístění stahování hello tooget pro hello nejnovější verzi. Ujistěte se, že architektura hello hello SAP NetWeaver knihovny (32bitová nebo 64bitová verze) odpovídá vaší instalace brány. Nainstalujte všechny soubory, které jsou součástí hello SAP NetWeaver RFC SDK podle toohello Poznámka SAP. Knihovna SAP NetWeaver Hello je také součástí hello instalace SAP Client Tools.

> [!TIP]
> Uveďte hello knihovny DLL z hello NetWeaver RFC SDK do složky system32 extrahovat.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API. 

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello. 
- Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. 

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. 
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. 

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázka s definicemi JSON entit služby Data Factory, které jsou používané toocopy data ze místní SAP Business Warehouse, najdete v části [JSON příklad: kopírování dat z SAP Business Warehouse tooAzure objektů Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) tohoto článku. 

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooan SAP BW úložiště dat:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis pro konkrétní tooSAP elementy JSON skladu obchodní (BW) propojené služby.

Vlastnost | Popis | Povolené hodnoty | Požaduje se
-------- | ----------- | -------------- | --------
server | Název hello serveru, na které hello SAP BW nachází instance. | Řetězec | Ano
systemNumber | Systém počet hello systému SAP BW. | Desetinné číslo letopočty řetězec. | Ano
clientId | ID klienta hello klienta v hello SAP W systému. | Tři číslice desítkové číslo řetězec. | Ano
uživatelské jméno | Název hello uživatele, který má přístup k serveru SAP toohello | Řetězec | Ano
heslo | Heslo pro uživatele hello. | Řetězec | Ano
gatewayName | Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní SAP BW instancí. | Řetězec | Ano
encryptedCredential | Hello šifrovaný řetězec přihlašovacích údajů. | Řetězec | Ne

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP BW hello typu **RelationalTable**. 


## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.

Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje SAP BW), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query | Určuje hello MDX dotazu tooread data z instance SAP BW hello. | Dotaz MDX. | Ano |


## <a name="json-example-copy-data-from-sap-business-warehouse-tooazure-blob"></a>Příklad JSON: kopírování dat z SAP Business Warehouse tooAzure objektů Blob
Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Tento příklad ukazuje, jak toocopy data ze tooan SAP Business Warehouse místní úložiště objektů Blob Azure. Však můžete zkopírovat data **přímo** tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.  

> [!IMPORTANT]
> Tato ukázka obsahuje fragmenty kódu JSON. Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.

Ukázka Hello má hello následující entity objektu pro vytváření dat:

1. Propojené služby typu [SapBw](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z tooan instance SAP Business Warehouse objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Jako první krok nastavte Brána pro správu dat hello. Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse propojená služba
Tato propojená služba propojuje SAP BW instance toohello datovou továrnu. Hello vlastnost Typ nastavena příliš**SapBw**. Hello rámci typeProperties část obsahuje informace o připojení pro SAP BW instanci hello. 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
Tato propojená služba propojuje účet úložiště Azure toohello datovou továrnu. Hello vlastnost Typ nastavena příliš**azurestorage**. Hello rámci typeProperties část obsahuje informace o připojení pro hello účet úložiště Azure.

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

### <a name="sap-bw-input-dataset"></a>Vstupní datové sady SAP BW
Tato datová sada definuje datovou sadu SAP Business Warehouse hello. Nastavte typ hello datovou sadu služby Data Factory hello příliš**RelationalTable**. V současné době nezadáte jakékoli vlastnosti specifické pro typ pro datové sadě služby SAP BW. Hello dotaz hello definici aktivity kopírování Určuje, jaké tooread data z instance SAP BW hello. 

Služba Data Factory hello nastavení tootrue vlastnost external informuje, že tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

Frekvence a intervalu vlastnosti definuje hello plán. V takovém případě hello je číst data z instance SAP BW hello každou hodinu. 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```



### <a name="azure-blob-output-dataset"></a>Výstupní datová sada Azure Blob
Tato datová sada definuje datovou sadu objektu Blob Azure výstup hello. Hello vlastnost Typ nastavena tooAzureBlob. Hello rámci typeProperties část obsahuje, které jsou uložená data hello zkopírovány z instance SAP BW hello. Hello data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a>Kanál s aktivitou kopírování
Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** (pro SAP BW zdroj) a **podřízený** je typ nastaven příliš**BlobSink**. Hello dotazu zadané pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a>Mapování typu pro SAP BW
Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:

1. Převod z typu too.NET typy nativní zdroje
2. Převést typ jímky toonative typ rozhraní .NET

Při přesouvání dat od SAP BW, se používají následující mapování hello z typů too.NET typy SAP BW.

Typ dat ve slovníku ABAP hello | Datový typ rozhraní .net
-------------------------------- | --------------
ACCP |  celá čísla
CHAR – | Řetězec
CLNT | Řetězec
AKTUÁLNÍ | Decimal
CUKY | Řetězec
DEC | Decimal
FLTP | Double
INT1 | Bajtů
INT2 | Int16
INT4 | celá čísla
JAZYK | Řetězec
LCHR | Řetězec
LRAW | Byte]
PREC | Int16
QUAN | Decimal
NEZPRACOVANÁ | Byte]
RAWSTRING | Byte]
ŘETĚZEC | Řetězec
JEDNOTKA | Řetězec
SOUBORY DAT | Řetězec
NUMC | Řetězec
TIMS | Řetězec

> [!NOTE]
> toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).


## <a name="map-source-toosink-columns"></a>Mapování zdrojových toosink sloupců
toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit. V tématu [Repeatable číst z relační zdrojů](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
