---
title: "aaaMove data ze serveru pomocí protokolu SFTP pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data z místní nebo serveru pomocí protokolu SFTP cloudu pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jingwang
ms.openlocfilehash: b976289e2c1b1899634bb5565b375499077fa554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Přesunutí dat ze serveru pomocí protokolu SFTP pomocí Azure Data Factory
Tento článek popisuje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z tooa serveru pomocí protokolu SFTP lokální/Cloudová podporované úložiště dat jímky. Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s kopie aktivity a hello seznamu úložiště dat, které jsou podporované jako zdroje nebo jímky.

Objekt pro vytváření dat v současné době podporuje pouze přesouvání dat od protokolu SFTP serveru tooother uloží, ale ne pro přesun dat z jiných dat ukládá tooan SFTP serveru. Podporuje místní a cloudové servery pomocí protokolu SFTP.

> [!NOTE]
> Aktivita kopírování neodstraní hello zdrojový soubor po úspěšně zkopírovaný toohello cílový. Pokud potřebujete toodelete hello zdrojový soubor po úspěšné kopie, vytvořte soubor vlastní aktivity toodelete hello a používat hello aktivitu v kanálu hello. 

## <a name="supported-scenarios-and-authentication-types"></a>Podporované scénáře a typy ověřování
Můžete použít tento SFTP konektor toocopy data z **i v cloudu pomocí protokolu SFTP servery a servery pomocí protokolu SFTP místní**. **Základní** a **parametru SshPublicKey** typy ověřování jsou podporovány při připojování serveru pomocí protokolu SFTP toohello.

Při kopírování dat z místního serveru pomocí protokolu SFTP, je nutné nainstalovat brána pro správu dat v hello místní prostředí nebo Azure virtuálních počítačů. V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o hello brány. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení hello brány a jeho použití.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z protokolu SFTP zdroje pomocí různých nástrojů nebo rozhraní API.

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

- Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. JSON – ukázky toocopy data z tooAzure serveru pomocí protokolu SFTP úložiště objektů Blob, najdete v části [JSON příklad: kopírování dat z objektu blob tooAzure serveru pomocí protokolu SFTP](#json-example-copy-data-from-sftp-server-to-azure-blob) tohoto článku.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooFTP propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- | --- |
| type | musí být nastavena vlastnost typu Hello příliš`Sftp`. |Ano |
| hostitele | Název nebo IP adresa serveru pomocí protokolu SFTP hello. |Ano |
| port |Port, na které hello SFTP server naslouchá. Hello výchozí hodnota je: 21 |Ne |
| authenticationType. |Zadejte typ ověřování. Povolené hodnoty: **základní**, **parametru SshPublicKey**. <br><br> Odkazovat příliš[základní ověřování pomocí](#using-basic-authentication) a [pomocí SSH ověření veřejného klíče](#using-ssh-public-key-authentication) částech na další vlastnosti a ukázky JSON v uvedeném pořadí. |Ano |
| skipHostKeyValidation | Zadejte, zda tooskip hostitele klíče ověřování. | Ne. Hello výchozí hodnota: false |
| hostKeyFingerprint | Zadejte hello prstu hello hostitele klíče. | Ano, pokud hello `skipHostKeyValidation` nastavena toofalse.  |
| gatewayName |Název hello Brána pro správu dat tooconnect tooan místní server pomocí protokolu SFTP. | Ano, pokud kopírování dat z místního serveru pomocí protokolu SFTP. |
| encryptedCredential | Šifrovaný přihlašovací údaj tooaccess hello SFTP server. Automaticky generovaný když zadáte základní ověřování (uživatelské jméno a heslo) nebo ověřování parametru SshPublicKey (uživatelské jméno + cesta privátního klíče nebo obsahu) v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog. | Ne. Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP. |

### <a name="using-basic-authentication"></a>Použití základního ověřování

toouse základní ověřování, nastavit `authenticationType` jako `Basic`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- | --- |
| uživatelské jméno | Uživatel, který má přístup toohello SFTP server. |Ano |
| heslo | Heslo pro uživatele hello (uživatelské jméno). | Ano |

#### <a name="example-basic-authentication"></a>Příklad: Základní ověřování
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Příklad: Základní ověřování s šifrovaném přihlašovacím údaji

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a>Pomocí ověření veřejného klíče SSH

nastavit toouse SSH ověření veřejného klíče, `authenticationType` jako `SshPublicKey`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- | --- |
| uživatelské jméno |Uživatel, který má přístup toohello SFTP serveru |Ano |
| privateKeyPath | Zadejte absolutní cestu toohello soubor privátního klíče můžete přístup k této brány. | Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`. <br><br> Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP. |
| privateKeyContent | Serializovaná řetězec hello privátní klíče obsahu. Průvodce kopírováním Hello můžete číst soubor privátního klíče hello a extrahování hello privátní klíče obsahu automaticky. Pokud používáte jakékoli jiné nástroje nebo SDK, použijte vlastnost privateKeyPath hello. | Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`. |
| přístupové heslo | Pokud soubor klíče hello je chráněn heslo, zadejte hello průchodu fráze nebo hesla toodecrypt hello privátní klíč. | Ano, pokud je chráněný soubor privátního klíče hello heslo. |

> [!NOTE]
> Konektor SFTP podporují pouze klíč OpenSSH. Ujistěte se, že je váš soubor klíče ve správném formátu hello. Můžete použít nástroj pro Putty tooconvert z .ppk tooOpenSSH formátu.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče filePath

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče obsahu

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu. Poskytuje informace, které je typu konkrétní toohello datovou sadu. rámci typeProperties Hello část datové sady typu **sdílení souborů** datová sada má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Dílčí cesta toohello složka. Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.<br/><br/>Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času. |Ano |
| fileName |Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: <br/><br/>Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| fileFilter |Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.<br/><br/>Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).<br/><br/>Příklady 1:`"fileFilter": "*.log"`<br/>Příklad 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter se vztahuje vstupní datové sady sdílení souborů. Tato vlastnost není podporována s HDFS. |Ne |
| partitionedBy |partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady. Například folderPath parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |
| useBinaryTransfer |Určit, jestli použít režim binární přenosu. Platí pro binárního režimu a false ASCII. Výchozí hodnota: True. Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp. |Ne |

> [!NOTE]
> Název souboru a fileFilter nelze použít současně.

### <a name="using-partionedby-property"></a>Pomocí vlastnosti partionedBy
Jak je uvedeno v předchozí části hello, můžete zadat dynamické folderPath, název souboru pro data časové řady s partitionedBy. Můžete tak učinit pomocí hello makra pro vytváření dat a hello systémové proměnné SliceStart, SliceEnd, který označuje hello logické časové období pro daný datový řez.

toolearn o datové sady času řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md) články.

#### <a name="sample-1"></a>Příklad 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
V tomto příkladu {řez} se nahradí hello hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu hello (YYYYMMDDHH) zadán. Hello SliceStart odkazuje toostart času řezu hello. Hello folderPath se liší pro každý řez. Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Příklad 2:

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
V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.

Zatímco se každý typ aktivity lišit podle vlastnosti hello k dispozici v rámci typeProperties části hello hello aktivity. Pro aktivitu kopírování vlastnosti typu hello lišit v závislosti na typech hello zdrojů a jímky.

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Podporované formáty souborů a komprese
V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.

## <a name="json-example-copy-data-from-sftp-server-tooazure-blob"></a>Příklad JSON: Kopírování dat z objektu blob tooAzure serveru pomocí protokolu SFTP
Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak zdroje dat toocopy z protokolu SFTP tooAzure úložiště objektů Blob. Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

> [!IMPORTANT]
> Tato ukázka obsahuje fragmenty kódu JSON. Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.

Ukázka Hello má hello následující entity objektu pro vytváření dat:

* Propojené služby typu [sftp](#linked-service-properties).
* Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).
* Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello kopíruje data ze serveru tooan protokolu SFTP objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**SFTP propojené služby**

Tento příklad používá hello základní ověřování s uživatelské jméno a heslo v prostém textu. Můžete také použít jednu z následujících způsobů hello:

* Základní ověřování s zašifrované přihlašovací údaje
* Ověření veřejného klíče SSH

V tématu [FTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
**Propojená služba Azure Storage**

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
**Vstupní datové sady pomocí protokolu SFTP**

Tato datová sada odkazuje složky pomocí protokolu SFTP toohello `mysharedfolder` a soubor `test.csv`. Hello kanál kopíruje cíl toohello souboru hello.

Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Výstupní datovou sadu objektů Blob v Azure**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kanál s aktivitou kopírování**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource** a **podřízený** je typ nastaven příliš**BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
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
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.

## <a name="next-steps"></a>Další kroky
V tématu hello následující články:

* [Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.
