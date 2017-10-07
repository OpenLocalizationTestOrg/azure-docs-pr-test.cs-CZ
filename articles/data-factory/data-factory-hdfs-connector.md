---
title: "aaaMove data z HDFS místní | Microsoft Docs"
description: "Další informace o tom, jak toomove data z místní HDFS pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 96387e5dd089099fc2e983ab26d67c2044b973b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Přesun dat z místní HDFS pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z místní službě HDFS. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

Může kopírovat data z úložiště dat podřízený HDFS tooany podporována. Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky. Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z úložiště dat tooother místní HDFS, ale není pro přesun dat z jiných dat úložiště tooan místní HDFS.

> [!NOTE]
> Aktivita kopírování neodstraní hello zdrojový soubor po úspěšně zkopírovaný toohello cílový. Pokud potřebujete toodelete hello zdrojový soubor po úspěšné kopie, vytvořte soubor vlastní aktivity toodelete hello a používat hello aktivitu v kanálu hello. 

## <a name="enabling-connectivity"></a>Povolení připojení
Služba data Factory podporuje pomocí hello Brána pro správu dat připojování tooon místní HDFS. V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány. Použijte hello brány tooconnect tooHDFS i v případě, že je hostovaná ve virtuálním počítači Azure IaaS.

> [!NOTE]
> Ujistěte se, hello Brána pro správu dat přístupný příliš**všechny** hello [název uzlu serveru]: [name portu uzlu] a [data uzlu servery]: [datový uzel port] clusteru Hadoop hello. Výchozí [název uzlu port] je 50070 a výchozí hodnota [datový uzel port] je 50075.

Když jste bránu nainstalovat na hello stejné místní počítač nebo virtuální počítač Azure hello jako hello HDFS, doporučujeme nainstalovat hello brány na samostatný počítač nebo Azure IaaS virtuálních počítačů. S brány na samostatný počítač snižuje kolize prostředků a zvyšuje výkon. Když instalujete na samostatný počítač hello brány, hello počítač měl být schopný tooaccess hello počítač s hello HDFS.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z HDFS zdroje pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z jiného úložiště dat HDFS, naleznete v tématu [JSON příklad: kopírování dat z místní HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) tohoto článku.

Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooHDFS:

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Propojená služba odkazuje data store tooa objekt pro vytváření dat. Vytvoření propojené služby typu **Hdfs** toolink služby místní HDFS tooyour data factory. Hello následující tabulka obsahuje popis JSON elementy konkrétní tooHDFS propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **Hdfs** |Ano |
| URL |Adresa URL toohello HDFS |Ano |
| authenticationType. |Anonymní, nebo Windows. <br><br> toouse **ověřování protokolem Kerberos** HDFS konektor, najdete v části příliš[v této části](#use-kerberos-authentication-for-hdfs-connector) tooset prostředí místní odpovídajícím způsobem. |Ano |
| Uživatelské jméno |Ověřování uživatelského jména pro systém Windows. |Ano (pro ověřování systému Windows) |
| heslo |Heslo pro ověřování systému Windows. |Ano (pro ověřování systému Windows) |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello HDFS. |Ano |
| encryptedCredential |[Nové AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) výstup hello přístup pověření. |Ne |

### <a name="using-anonymous-authentication"></a>Pomocí anonymní ověřování

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a>Pomocí ověřování systému Windows

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a>Vlastnosti datové sady
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **sdílení souborů** (což zahrnuje datová sada HDFS) má následující vlastnosti hello

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Složka toohello cesta. Příklad:`myfolder`<br/><br/>Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello. Příklad: pro folder\subfolder, určete složku\\\\podsložky a pro d:\samplefolder, zadejte d:\\\\ukázková_složka.<br/><br/>Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času. |Ano |
| fileName |Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: <br/><br/>Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| partitionedBy |partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady. Příklad: folderPath parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

> [!NOTE]
> Název souboru a fileFilter nelze použít současně.

### <a name="using-partionedby-property"></a>Pomocí vlastnosti partionedBy
Jak je uvedeno v předchozí části hello, můžete zadat dynamické folderPath a název souboru pro data časové řady s hello **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné hello](data-factory-functions-variables.md).

toolearn Další informace o datové sady času řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md) články.

#### <a name="sample-1"></a>Příklad 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
V tomto příkladu {řez} se nahradí hello hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu hello (YYYYMMDDHH) zadán. Hello SliceStart odkazuje toostart času řezu hello. Hello folderPath se liší pro každý řez. Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Příklad 2:

```JSON
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

Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.

Pro aktivitu kopírování, pokud je zdroj typu **FileSystemSource** hello následující vlastnosti jsou k dispozici v rámci typeProperties části:

**FileSystemSource** podporuje hello následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky. |Hodnota TRUE, False (výchozí) |Ne |

## <a name="supported-file-and-compression-formats"></a>Podporované formáty souborů a komprese
V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.

## <a name="json-example-copy-data-from-on-premises-hdfs-tooazure-blob"></a>Příklad JSON: kopírování dat z místní HDFS tooAzure objektů Blob
Tento příklad ukazuje, jak toocopy data z HDFS tooAzure místní úložiště objektů Blob. Však můžete zkopírovat data **přímo** tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.  

Ukázka Hello poskytuje JSON definice hello následující entity služby Data Factory. Můžete použít tyto definice toocreate kanálu toocopy data z HDFS tooAzure úložiště objektů Blob pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).

1. Propojené služby typu [OnPremisesHdfs](#linked-service-properties).
2. Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Ukázka Hello zkopíruje data z HDFS tooan místní objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

Jako první krok nastavte Brána pro správu dat hello. Hello pokyny v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.

**Propojená služba HDFS:** tento příklad používá hello ověřování systému Windows. V tématu [HDFS propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

**Propojená služba Azure Storage:**

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

**HDFS vstupní datovou sadu:** tato datová sada odkazuje toohello HDFS složky DataTransfer/UnitTest /. kanál Hello zkopíruje všechny soubory hello v této složce toohello cíli.

Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Azure Blob výstupní datovou sadu:**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Aktivita kopírování v kanálu pomocí systému souborů zdrojový a podřízený objekt Blob:**

Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Ověřování pomocí protokolu Kerberos pro HDFS konektor
Existují dvě možnosti tooset hello místní prostředí, jako toouse ověřování protokolem Kerberos v HDFS konektoru. Můžete vybrat, zda text hello, jeden lépe vyhovuje váš případ.
* Možnost 1: [připojení počítače brány ve sféře Kerberos](#kerberos-join-realm)
* Možnost 2: [povolit vzájemné vztah důvěryhodnosti mezi doménou systému Windows a Sféra Kerberos](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Možnost 1: Připojení počítače brány ve sféře Kerberos

#### <a name="requirement"></a>Požadavek:

* počítač brány Hello potřebuje Sféra Kerberos hello toojoin a nemůže připojit k žádné doméně systému Windows.

#### <a name="how-tooconfigure"></a>Jak tooconfigure:

**Na počítači brány:**

1.  Spustit hello **Ksetup** tooconfigure nástroj hello serveru pomocí protokolu Kerberos služby KDC a sféru.

    Hello počítače musí být nakonfigurován jako člena pracovní skupiny, protože Sféra Kerberos se liší od domény systému Windows. Toho lze dosáhnout nastavením Sféra Kerberos hello a takto přidejte server služby KDC. Nahraďte *REALM.COM* s vlastními příslušných sféry podle potřeby.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Restartujte** hello počítač po spuštění těchto příkazů 2.

2.  Ověření konfigurace hello s **Ksetup** příkaz. výstup Hello by měl vypadat podobně jako:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**V Azure Data Factory:**

* Konfigurovat pomocí konektoru hello HDFS **ověřování systému Windows** společně s protokolem Kerberos hlavní název a heslo tooconnect toohello HDFS zdroje dat. Zkontrolujte [vlastnosti propojené služby HDFS](#linked-service-properties) části na podrobnosti o konfiguraci.

### <a name="kerberos-mutual-trust"></a>Možnost 2: Povolení vzájemné vztah důvěryhodnosti mezi doménou systému Windows a Sféra Kerberos

#### <a name="requirement"></a>Požadavek:
*   počítač brány Hello musí připojit k doméně systému Windows.
*   Je nutné nastavení oprávnění tooupdate hello řadiče domény.

#### <a name="how-tooconfigure"></a>Jak tooconfigure:

> [!NOTE]
> V následujícím kurzu s vlastními příslušných sféry a řadič domény, podle potřeby hello nahraďte REALM.COM a AD.COM.

**Na serveru služby KDC:**

1.  Upraví konfiguraci služby KDC hello v **krb5.conf** toolet souboru služby KDC vztahu důvěryhodnosti domény systému Windows odkazující toohello následující konfigurace šablony. Ve výchozím nastavení, konfiguraci hello nachází ve **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  **Restartujte** hello služba KDC po konfiguraci.

2.  Příprava objekt zabezpečení s názvem  **krbtgt/REALM.COM@AD.COM**  na serveru služby KDC s hello následující příkaz:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  V **hadoop.security.auth_to_local** konfigurace služby HDFS souboru, přidejte `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Na řadiči domény:**

1.  Spusťte následující hello **Ksetup** příkazy tooadd položku sféry:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Navázání vztahu důvěryhodnosti z domény systému Windows tooKerberos sféry. [heslo] je hello heslo pro objekt zabezpečení hello  **krbtgt/REALM.COM@AD.COM** .

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Vyberte šifrovací algoritmus použitý v protokolu Kerberos.

    1. Přejděte tooServer Manager > Správa zásad skupiny > domény > objekty zásad skupiny > výchozí nebo aktivní zásady domény a úpravy.

    2. V hello **Editor správy zásad skupiny** automaticky otevíraném okně, přejděte tooComputer konfigurace > zásady > Nastavení systému Windows > Nastavení zabezpečení > Místní zásady > Možnosti zabezpečení a nakonfigurujte **sítě zabezpečení: Konfigurace typy šifrování, které jsou povoleny pro protokol Kerberos**.

    3. Vyberte hello šifrovací algoritmus toouse chcete při připojení tooKDC. Běžně můžete jednoduše vybrat všechny možnosti hello.

        ![Typy šifrování konfigurace pro protokol Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Použití **Ksetup** příkaz toospecify hello šifrovací algoritmus toobe použít na hello konkrétní SFÉRY.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Vytvořte objekt, v pořadí toouse Kerberos hlavní v doméně systému Windows hello mapování mezi hello doménový účet a protokolu Kerberos.

    1. Spuštění nástroje pro správu hello > **Active Directory Users and Computers**.

    2. Konfigurace rozšířených funkcí kliknutím **zobrazení** > **pokročilé funkce**.

    3. Vyhledejte hello účet toowhich chcete toocreate mapování a klikněte pravým tlačítkem na tooview **mapování názvů** > klikněte na tlačítko **Kerberos názvy** kartě.

    4. Přidáte objekt z hello sféry.

        ![Mapování Identity zabezpečení](media/data-factory-hdfs-connector/map-security-identity.png)

**Na počítači brány:**

* Spusťte následující hello **Ksetup** příkazy tooadd položku sféry.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**V Azure Data Factory:**

* Konfigurovat pomocí konektoru hello HDFS **ověřování systému Windows** společně s účet domény nebo zdroj dat HDFS toohello tooconnect objekt zabezpečení protokolu Kerberos. Zkontrolujte [vlastnosti propojené služby HDFS](#linked-service-properties) části na podrobnosti o konfiguraci.

> [!NOTE]
> toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).


## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.
