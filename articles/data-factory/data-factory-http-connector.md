---
title: aaaMove data ze zdroje HTTP - Azure | Microsoft Docs
description: "Informace o tom, jak zdroje dat toomove z místní nebo cloudové HTTP pomocí Azure Data Factory."
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
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: e39b9cbff870aef4be91938cacff39a2fd12d64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a>Přesun dat z HTTP zdroje pomocí Azure Data Factory
Tento článek popisuje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z tooa koncový bod HTTP lokální/Cloudová podporované úložiště dat jímky. Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s kopie aktivity a hello seznamu úložiště dat, které jsou podporované jako zdroje nebo jímky.

Objekt pro vytváření dat aktuálně podporuje pouze přesun dat z protokolu HTTP zdroje tooother datová úložiště, ale není přesouvání dat od ostatních dat ukládá cílové tooan HTTP.

## <a name="supported-scenarios-and-authentication-types"></a>Podporované scénáře a typy ověřování
Můžete použít tento HTTP konektor tooretrieve data z **cloudové i místní koncový bod HTTP/s** pomocí protokolu HTTP **získat** nebo **POST** metoda. jsou podporovány následující typy ověřování Hello: **anonymní**, **základní**, **Digest**, **Windows**, a  **ClientCertificate**. Všimněte si hello rozdíl mezi tento konektor a hello [konektor tabulky webových](data-factory-web-table-connector.md) je: hello druhé je použité tooextract tabulku obsahu z webové stránky HTML.

Při kopírování dat z místní koncový bod protokolu HTTP, je nutné nainstalovat brána pro správu dat v hello místní prostředí nebo Azure virtuálních počítačů. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje HTTP pomocí různých nástrojů nebo rozhraní API.

- Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

- Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování. JSON – ukázky toocopy data z tooAzure HTTP zdroj úložiště objektů Blob, najdete v části [JSON příklady](#json-examples) části Tento článek.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Hello následující tabulka obsahuje popis JSON elementy konkrétní tooHTTP propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type | vlastnost typu Hello musí být nastavena na: `Http`. | Ano |
| Adresa URL | Základní adresa URL toohello webového serveru | Ano |
| authenticationType. | Určuje typ ověřování hello. Povolené hodnoty jsou: **anonymní**, **základní**, **Digest**, **Windows**, **ClientCertificate**. <br><br> V uvedeném pořadí odkazovat toosections dál v této tabulce na další vlastnosti a ukázky JSON pro tyto typy ověřování. | Ano |
| enableServerCertificateValidation | Zadejte, zda, tooenable serveru SSL ověření certifikátu, pokud je zdroj HTTPS webového serveru | Ne, výchozí hodnota je true |
| gatewayName | Název hello Brána pro správu dat tooconnect tooan místní zdroje pomocí protokolu HTTP. | Ano, pokud kopírování dat z místního zdroje HTTP. |
| encryptedCredential | Šifrovaný přihlašovací údaj tooaccess hello koncový bod HTTP. Automaticky vygenerované při konfiguraci hello ověřovací informace v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog. | Ne. Platí jenom v případě, že kopírování dat z místního serveru HTTP. |

V tématu [přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o nastavení přihlašovacích údajů pro zdroj dat konektor místní HTTP.

### <a name="using-basic-digest-or-windows-authentication"></a>Pomocí ověřování Basic, ověřování algoritmem Digest nebo systému Windows

Nastavit `authenticationType` jako `Basic`, `Digest`, nebo `Windows`a zadejte následující vlastnosti kromě hello HTTP konektor obecné těch, které jsou zavedené výše hello:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| uživatelské jméno | Uživatelské jméno tooaccess hello koncový bod HTTP. | Ano |
| heslo | Heslo pro uživatele hello (uživatelské jméno). | Ano |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Příklad: použití ověřování Basic, ověřování algoritmem Digest nebo systému Windows

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Pomocí ClientCertificate ověřování

toouse základní ověřování, nastavit `authenticationType` jako `ClientCertificate`a zadejte následující vlastnosti kromě hello HTTP konektor obecné těch, které jsou zavedené výše hello:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| embeddedCertData | Hello obsah kódováním Base64 binárních dat souboru hello Personal Information Exchange (PFX). | Zadejte buď hello `embeddedCertData` nebo `certThumbprint`. |
| CertThumbprint | Hello kryptografický otisk certifikátu hello, který byl nainstalován v úložišti certifikátů počítače brány. Platí jenom v případě, že kopírování dat z místního zdroje HTTP. | Zadejte buď hello `embeddedCertData` nebo `certThumbprint`. |
| heslo | Heslo přidružené k hello certifikátu. | Ne |

Pokud používáte `certThumbprint` pro ověřování a hello certifikát nainstalován v osobním úložišti hello hello místního počítače, je třeba služba brány pro toohello toogrant hello oprávnění ke čtení:

1. Spusťte konzolu Microsoft Management Console (MMC). Přidat hello **certifikáty** modul snap-in tohoto cíle hello **místního počítače**.
2. Rozbalte položku **certifikáty**, **osobní**a klikněte na tlačítko **certifikáty**.
3. Klikněte pravým tlačítkem na certifikát hello z osobního úložiště hello a vyberte **všechny úlohy**->**spravovat privátní klíče...**
3. Na hello **zabezpečení** přidejte hello uživatelský účet, pod kterým běží hostitelská služba brány správy dat s certifikátem toohello hello přístup pro čtení.  

#### <a name="example-using-client-certificate"></a>Příklad: pomocí klientského certifikátu
Tato propojená služba propojuje vaše data factory tooan místní HTTP webový server. Používá klientský certifikát, který je nainstalován na počítači hello s brána správy dat nainstalována.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Příklad: pomocí klientského certifikátu do souboru
Tato propojená služba propojuje vaše data factory tooan místní HTTP webový server. Soubor certifikátu klienta na počítači hello používá s brána správy dat nainstalována.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **Http** má následující vlastnosti hello

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Zadaný typ hello hello datovou sadu. musí být nastaven příliš`Http`. | Ano |
| relativeUrl | Relativní adresa URL toohello prostředek obsahující hello data. Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby. <br><br> tooconstruct dynamické adresy URL, můžete použít [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md), například "relativeUrl": "$$Text.Format ('/ my/sestavy? měsíc = {0:yyyy}-{0:MM} & fmt = csv', SliceStart)". | Ne |
| requestMethod | Metoda HTTP. Povolené hodnoty jsou **získat** nebo **POST**. | Ne. Výchozí hodnota je `GET`. |
| additionalHeaders | Další hlavičky žádosti HTTP. | Ne |
| RequestBody | Text pro požadavek HTTP. | Ne |
| Formát | Pokud chcete, aby toosimply **načtení dat hello z koncový bod HTTP jako-je** bez analýza ho, přeskočte tento formát nastavení. <br><br> Pokud chcete tooparse hello HTTP odpovědi obsahu během kopírování, jsou podporovány následující typy formátu hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

### <a name="example-using-hello-get-default-method"></a>Příklad: pomocí hello metodu GET (výchozí)

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-hello-post-method"></a>Příklad: pomocí metody POST hello

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
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

Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivit na hello se každý typ aktivity lišit podle druhé straně. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

V současné době po hello zdroj v aktivitě kopírování typu **HttpSource**, jsou podporovány následující vlastnosti hello.

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| httpRequestTimeout | Hello vypršení časového limitu (časový interval) pro tooget požadavku HTTP hello odpověď. Je hello časový limit tooget odpověď, nebyla hello časový limit tooread data odpovědi. | Ne. Výchozí hodnota: 00:01:40 |

## <a name="supported-file-and-compression-formats"></a>Podporované formáty souborů a komprese
V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.

## <a name="json-examples"></a>Příklady JSON
Následující ukázka Hello poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak zdroje dat toocopy z HTTP tooAzure úložiště objektů Blob. Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

### <a name="example-copy-data-from-http-source-tooazure-blob-storage"></a>Příklad: Kopírování dat z tooAzure HTTP zdroj úložiště objektů Blob
Hello řešení Data Factory pro tato ukázka obsahuje hello následující entity služby Data Factory:

1. Propojené služby typu [HTTP](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [Http](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [HttpSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z tooan zdroje HTTP objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

### <a name="http-linked-service"></a>Služba HTTP propojené
Tento příklad, že používá hello HTTP propojená služba s anonymní ověřování. V tématu [HTTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
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

### <a name="http-input-dataset"></a>Vstupní datové sady HTTP
Nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a>Výstupní datová sada Azure Blob

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).

```JSON
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

### <a name="pipeline-with-copy-activity"></a>Kanál s aktivitou kopírování

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**HttpSource** a **podřízený** je typ nastaven příliš**BlobSink**.

V tématu [HttpSource](#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem hello HttpSource.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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

> [!NOTE]
> toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
