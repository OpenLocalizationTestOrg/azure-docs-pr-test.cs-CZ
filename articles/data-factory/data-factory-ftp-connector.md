---
title: "aaaMove dat ze serveru FTP pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove dat ze serveru FTP pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jingwang
ms.openlocfilehash: c707e29532b2a8a870603948cb6150ab857bd6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Přesun dat ze serveru FTP pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivita kopírování v Azure Data Factory toomove data ze serveru FTP. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Můžete zkopírovat data z úložiště dat tooany podporované podřízený FTP serveru. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat se aktuálně podporuje pouze přesunutí dat od FTP server tooother ukládá, ale není přesouvání dat od ostatních dat ukládá tooan FTP serveru. Podporuje místní a cloudové servery FTP.

> [!NOTE]
> Aktivita kopírování Hello neodstraní hello zdrojový soubor po úspěšně zkopírovaný toohello cílový. Pokud potřebujete toodelete hello zdrojový soubor po úspěšné kopie, vytvořte soubor vlastní aktivity toodelete hello a použít hello aktivity v kanálu hello. 

## <a name="enable-connectivity"></a>Povolit připojení
Pokud přesouváte data ze **místní** úložiště dat cloudu tooa server FTP (například tooAzure úložiště objektů Blob), nainstalovat a používat Brána pro správu dat. Hello Brána pro správu dat je klientský agent, který je nainstalován v místním počítači a umožňuje cloudové služby tooconnect tooan místnímu prostředku. Podrobnosti najdete v tématu [Brána pro správu dat](data-factory-data-management-gateway.md). Podrobné pokyny k nastavení hello brány a jeho použití naleznete v tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md). Používáte server hello brány tooconnect tooan FTP, i když je hello server na Azure infrastruktury jako služby (IaaS) virtuální počítač (VM).

Je možné tooinstall hello brány hello stejné na místní počítač nebo virtuální počítač IaaS jako hello serveru FTP. Doporučujeme však nainstalovat bránu hello na samostatný počítač nebo sporu prostředků tooavoid virtuálních počítačů IaaS a pro dosažení vyššího výkonu. Když instalujete na samostatný počítač hello brány, hello počítač měl být schopný tooaccess hello FTP serveru.

## <a name="get-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje FTP pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním služby Data Factory**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé návod.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.

Jestli používáte nástroje hello nebo rozhraní API, proveďte následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Pokud používáte rozhraní API (s výjimkou .NET API) nebo nástroje, definujete tyto entity služby Data Factory pomocí formátu JSON hello. Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z úložiště dat služby FTP, naleznete v tématu hello [JSON příklad: kopírování dat z objektu blob tooAzure serveru FTP](#json-example-copy-data-from-ftp-server-to-azure-blob) tohoto článku.

> [!NOTE]
> Podrobnosti o podporovaných toouse formáty souborů a komprese najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooFTP.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka popisuje JSON elementy konkrétní tooan FTP propojené služby.

| Vlastnost | Popis | Požaduje se | Výchozí |
| --- | --- | --- | --- |
| type |Nastavte tento tooFtpServer. |Ano |&nbsp; |
| hostitele |Zadejte hello název nebo IP adresu serveru FTP hello. |Ano |&nbsp; |
| authenticationType. |Zadejte typ ověřování hello. |Ano |Anonymní, základní |
| uživatelské jméno |Zadejte hello uživatel, který má přístup k serveru FTP toohello. |Ne |&nbsp; |
| heslo |Zadejte hello heslo pro uživatele hello (uživatelské jméno). |Ne |&nbsp; |
| encryptedCredential |Zadejte hello šifrovat přihlašovací údaje tooaccess hello FTP serveru. |Ne |&nbsp; |
| gatewayName |Zadejte název hello hello brány Brána pro správu dat tooconnect tooan na místním serveru FTP serveru. |Ne |&nbsp; |
| port |Zadejte hello port, na které hello FTP server naslouchá. |Ne |21 |
| enableSsl |Zadejte, zda toouse FTP přes kanál SSL/TLS. |Ne |Hodnota TRUE |
| enableServerCertificateValidation |Zadejte, zda tooenable server SSL certifikát ověření, pokud používáte FTP přes kanál SSL/TLS. |Ne |Hodnota TRUE |

### <a name="use-anonymous-authentication"></a>Anonymní ověřování použijte

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a>Použít uživatelské jméno a heslo v prostém textu pro základní ověřování

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a>Použijte port, enableSsl, enableServerCertificateValidation

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a>Použití encryptedCredential pro ověřování a brány

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md). Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu. Poskytuje informace, které je typu konkrétní toohello datovou sadu. Hello **rámci typeProperties** části datové sady typu **sdílení souborů** má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Složka toohello cestou. Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.<br/><br/>Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez začínat a končit hodnoty data a času. |Ano |
| fileName |Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.<br/><br/>Když **fileName** není zadané pro datovou sadu výstupů, hello název souboru hello generována je v hello následující formát: <br/><br/>Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Ne |
| fileFilter |Zadejte filtru toobe používá tooselect podmnožinu souborů v hello **folderPath**, ne všechny soubory.<br/><br/>Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).<br/><br/>Příklad 1:`"fileFilter": "*.log"`<br/>Příklad 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** lze použít pro datové sadě služby vstupní sdílení souborů. Tato vlastnost není podporována s Hadoop Distributed File System (HDFS). |Ne |
| partitionedBy |Použít toospecify dynamický **folderPath** a **fileName** pro data časové řady. Například můžete zadat **folderPath** , je pro každou hodinu dat parametry. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu hello [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formátu ](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete soubory toocopy, protože se jedná o mezi souborové úložiště (binární kopie), část formátu hello v obou definice vstupní a výstupní datovou sadu přeskočte. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**, a jsou podporované úrovně **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |
| useBinaryTransfer |Určete, zda toouse hello binární přenášet režimu. Hello hodnoty jsou pro binárním režimu (Toto je výchozí hodnota hello) na hodnotu true a false pro ASCII. Tuto vlastnost lze použít pouze v případě hello typu přidružené propojené služby typu: Server_ftp. |Ne |

> [!NOTE]
> **Název souboru** a **fileFilter** nelze používat současně.

### <a name="use-hello-partionedby-property"></a>Použijte vlastnost partionedBy hello
Jak je uvedeno v předchozí části hello, můžete zadat dynamický **folderPath** a **fileName** pro data časové řady s hello **partitionedBy** vlastnost.

toolearn o datové sady času řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Ukázka 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
V tomto příkladu {řez} se nahradí hello hodnoty proměnné objektu pro vytváření dat systému SliceStart, v hello formátu zadaný (YYYYMMDDHH). Hello SliceStart odkazuje toostart času řezu hello. Cesta ke složce Hello se liší pro každý řez. (Například wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.)

#### <a name="sample-2"></a>Ukázka 2

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
V tomto příkladu se extrahují hello rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány hello **folderPath** a **fileName** vlastnosti.

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md). Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivity na hello druhé straně, se liší podle každý typ aktivity. Pro aktivitu kopírování hello vlastnosti typu hello lišit v závislosti na typech hello zdrojů a jímky.

Při aktivitě kopírování, pokud je zdroj hello typu **FileSystemSource**, hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z podsložky hello nebo pouze z hello zadané složky. |Hodnota TRUE, False (výchozí) |Ne |

## <a name="json-example-copy-data-from-ftp-server-tooazure-blob"></a>Příklad JSON: kopírování dat z tooAzure serveru FTP objektů Blob
Tento příklad ukazuje, jak toocopy data z FTP serveru tooAzure úložiště objektů Blob. Ale data se dají zkopírovat přímo tooany hello jímky stanovené v hello [podporované úložiště dat a formáty](data-factory-data-movement-activities.md#supported-data-stores-and-formats), a to pomocí aktivity kopírování hello v datové továrně.  

Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):

* Propojené služby typu [Server_ftp](#linked-service-properties)
* Propojené služby typu [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)
* Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties)
* Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

Ukázka Hello kopíruje data ze FTP server tooan objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

### <a name="ftp-linked-service"></a>Propojená služba FTP

Tento příklad používá základní ověřování s hello uživatelské jméno a heslo v prostém textu. Můžete také použít jednu z následujících způsobů hello:

* Anonymní ověřování
* Základní ověřování s zašifrované přihlašovací údaje
* FTP přes SSL/TLS (FTPS)

V tématu hello [FTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
    }
  }
}
```
### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage

```JSON
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
### <a name="ftp-input-dataset"></a>FTP vstupní datové sady

Tato datová sada odkazuje toohello FTP složky `mysharedfolder` a soubor `test.csv`. Hello kanál kopíruje cíl toohello souboru hello.

Nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a>Výstupní datová sada Azure Blob

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello se dynamicky vyhodnotí, podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá hello rok, měsíc, den a čas části hello počáteční čas.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>Aktivita kopírování v kanálu s podřízený zdroj a objektů blob systému souborů

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady, a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource**a hello **podřízený** je typ nastaven příliš**BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="next-steps"></a>Další kroky
V tématu hello následující články:

* toolearn o klíči faktory, že dopad výkonu (aktivita kopírování) přesun dat v datové továrně a různé způsoby toooptimize, najdete v části hello [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md).

* Podrobné pokyny pro vytvoření kanálu s aktivitou kopírování najdete v tématu hello [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
