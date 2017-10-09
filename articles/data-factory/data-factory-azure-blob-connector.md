---
title: aaaCopy data do/z Azure Blob Storage | Microsoft Docs
description: "Zjistěte, jak toocopy blob dat v Azure Data Factory. Použijte naše ukázka: jak toocopy tooand dat z Azure Blob Storage a Azure SQL Database."
keywords: "data objektu BLOB, kopírovat objekt blob systému azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 8428c64e8e8b1084b3f2f680c4e1819559e4ffa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooor-from-azure-blob-storage-using-azure-data-factory"></a>Tooor kopírování dat z Azure Blob Storage pomocí Azure Data Factory
Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toocopy data tooand z Azure Blob Storage. Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.

## <a name="overview"></a>Přehled
Data můžete zkopírovat z jakéhokoli podporované zdroje dat ukládání tooAzure úložiště objektů Blob nebo z Azure Blob Storage tooany podporované jímku dat úložiště. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje nebo jímky aktivitou kopírování hello. Například můžete přesunout data **z** databázi systému SQL Server nebo Azure SQL database **k** Azure blob storage. A může kopírovat data **z** úložiště objektů blob Azure **k** Azure SQL Data Warehouse nebo kolekci Azure Cosmos DB. 

## <a name="supported-scenarios"></a>Podporované scénáře
Může kopírovat data **z Azure Blob Storage** toohello následující úložišť dat:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Data můžete zkopírovat z hello následující úložišť dat **tooAzure úložiště objektů Blob**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> Aktivita kopírování podporuje kopírování dat z / tooboth účtů úložiště pro obecné účely Azure a Hot/Cool úložiště objektů Blob. Aktivita Hello podporuje **čtení z bloku, připojte, nebo objekty BLOB stránky**, ale podporuje **zápis objekty BLOB bloku tooonly**. Azure Storage úrovně Premium není podporován jako jímka, protože je zálohovaný díky objekty BLOB stránky.
> 
> Aktivita kopírování nedojde k odstranění dat ze zdroje hello po hello, data se úspěšně zkopíroval toohello cílový. Pokud potřebujete po úspěšné kopie toodelete zdrojová data, vytvořte [vlastní aktivity](data-factory-use-custom-activities.md) toodelete hello dat a používání hello aktivitu v kanálu hello. Příklad najdete v tématu hello [odstranění objektů blob nebo složky ukázce na Githubu](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity). 

## <a name="get-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Blob Storage pomocí různých nástrojů nebo rozhraní API.

Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**. Tento článek má [návod](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) pro vytváření dat kanál toocopy ze Azure Blob Storage umístění tooanother umístění úložiště objektů Blob Azure. Kurz týkající se vytváření kanálu toocopy data ze Azure Blob Storage tooAzure SQL Database, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).

Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**. V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.

Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:

1. Vytvoření **objekt pro vytváření dat**. Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů. 
2. Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory. Například pokud kopírování dat z Azure SQL database tooan úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink vašeho účtu úložiště Azure a Azure SQL databáze tooyour data factory. Vlastnosti propojené služby, které jsou specifické tooAzure úložiště objektů Blob, najdete v části [propojené vlastnosti služby](#linked-service-properties) části. 
2. Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování. V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello. A vytvořte jinou datovou sadu toospecify hello SQL tabulkou v hello Azure SQL database, která obsahuje hello daty zkopírovanými z úložiště objektů blob hello. Vlastnosti datové sady, které jsou specifické tooAzure úložiště objektů Blob, najdete v části [vlastnosti datové sady](#dataset-properties) části.
3. Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup. V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlSink jako jímku pro aktivitu kopírování hello. Podobně pokud zkopírujete z Azure SQL Database tooAzure úložiště objektů Blob, použijte SqlSource a BlobSink v aktivitě kopírování hello. Vlastnosti aktivity kopírování, které jsou specifické tooAzure úložiště objektů Blob, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části. Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.  

Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello). Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.  Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do/z Azure Blob Storage, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-blob-storage  ) tohoto článku.

Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure úložiště objektů Blob.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby
Existují dva typy propojené služby můžete použít toolink služby Azure Storage tooan Azure data factory. Jsou: **azurestorage** propojená služba a **AzureStorageSas** propojené služby. Hello propojené služby Azure Storage poskytuje objekt pro vytváření dat hello s globálním přístupem toohello Azure Storage. Zatímco hello Azure úložiště SAS (sdíleného přístupového podpisu) propojená služba poskytuje objekt pro vytváření dat hello s přístup omezený nebo časově vázaných toohello Azure Storage. Nejsou žádné další rozdíly mezi tyto dvě propojené služby. Zvolte hello propojené služby, který vyhovuje vašim potřebám. Hello následující oddíly poskytují další podrobnosti o tyto dvě propojené služby.

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Vlastnosti datové sady
toospecify toorepresent datové sady vstupních nebo výstupních dat v Azure Blob Storage, nastavte vlastnost typu hello hello datovou sadu, která: **AzureBlob**. Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Azure Storage nebo Azure úložiště SAS propojené služby.  vlastnosti typu Hello sady dat hello zadejte hello **kontejner objektů blob** a hello **složky** v úložišti objektů blob hello.

Úplný seznam části JSON & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku. Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).

Objekt pro vytváření dat podporuje následující hodnoty kompatibilní se specifikací CLS .NET na základě typu pro poskytnutí informací o typu "struktury" zdroje dat schématu na čtení jako Azure blob hello: Int16, Int32, Int64, jeden, Double, Decimal, Byte [], Bool, String, Guid, Datetime, DateTimeOffset, Timespan. Objekt pro vytváření dat převody typů automaticky provede při přesunu, že data ze zdrojových dat úložiště tooa podřízený data.

Hello **rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění hello formátu atd, hello dat v úložišti dat hello. rámci typeProperties Hello část datové sady typ **AzureBlob** datová sada má hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Cesta toohello kontejneru a složce v úložišti objektů blob hello. Příklad: myblobcontainer\myblobfolder\ |Ano |
| fileName |Název objektu hello blob. Název souboru je volitelné a velká a malá písmena.<br/><br/>Pokud určíte název souboru, hello aktivitu (včetně kopie) funguje na hello konkrétní objekt Blob.<br/><br/>Pokud není zadán název souboru, zahrnuje kopírování všech objektů BLOB v hello folderPath pro vstupní datové sady.<br/><br/>Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v aktivity podřízený hello název souboru hello generované by v hello následující tento formát: Data.<Guid>. TXT (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| partitionedBy |partitionedBy vlastnost je volitelná. Můžete ho toospecify dynamické folderPath a název souboru pro data časové řady. Například folderPath lze nastavit parametry pro každou hodinu data. V tématu hello [pomocí části vlastnost partitionedBy](#using-partitionedBy-property) podrobnosti a příklady. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

### <a name="using-partitionedby-property"></a>Pomocí vlastnost partitionedBy
Jak je uvedeno v předchozí části hello, můžete zadat dynamické folderPath a název souboru pro data časové řady s hello **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné hello](data-factory-functions-variables.md).

Další informace o datové sady času řady, plánování a řezy najdete v tématu [vytváření datových sad](data-factory-create-datasets.md) a [plánování a provádění](data-factory-scheduling-and-execution.md) články.

#### <a name="sample-1"></a>Ukázka 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

V tomto příkladu {řez} se nahradí hello hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu hello (YYYYMMDDHH) zadán. Hello SliceStart odkazuje toostart času řezu hello. Hello folderPath se liší pro každý řez. Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104

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

V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit
Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku. Vlastnosti, například název, popis, vstupní a výstupní datové sady a zásad jsou dostupné pro všechny typy aktivit. Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity. Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky. Pokud přesouváte data z Azure Blob Storage, nastavíte typ zdroje hello v aktivitě kopírování hello příliš**BlobSource**. Podobně pokud přesouváte data tooan Azure Blob Storage, nastavíte typ jímky hello v aktivitě kopírování hello příliš**BlobSink**. Tato část obsahuje seznam vlastností, které jsou podporované BlobSource a BlobSink.

**BlobSource** podporuje následující vlastnosti v hello hello **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky. |True (výchozí hodnota), False. |Ne |

**BlobSink** podporuje následující vlastnosti hello **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| copyBehavior |Definuje chování kopie hello, pokud je zdroj hello BlobSource nebo systému souborů. |<b>PreserveHierarchy</b>: uchovává hello hierarchií souborů v cílové složce hello. relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.<br/><br/><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello jsou v hello první úroveň cílové složce. Hello zaměřením mít název automaticky generovány. <br/><br/><b>MergeFiles</b>: sloučí všechny soubory ze hello zdrojové složky tooone souboru. Pokud je zadán hello název souboru nebo objekt Blob, název sloučené souboru hello by být zadaný název hello; jinak by automaticky generovaný soubor název. |Ne |

**BlobSource** také podporuje tyto dvě vlastnosti z důvodu zpětné kompatibility.

* **treatEmptyAsNull**: Určuje, zda tootreat hodnotu null nebo prázdný řetězec jako hodnotu null.
* **skipHeaderLineCount** -Určuje, kolik řádků potřebovat přeskočen. Vztahuje se pouze pokud vstupní datové sady používá TextFormat.

Podobně **BlobSink** podporuje hello následující vlastnosti pro zpětnou kompatibilitu.

* **blobWriterAddHeader**: Určuje, zda tooadd hlavičku definice sloupců při zápisu tooan výstupní datovou sadu.

Datové sady nyní podporu hello následující vlastnosti, které implementují hello stejnou funkci: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

Hello následující tabulka obsahuje informace o použití hello nové vlastnosti datové sady místo tyto vlastnosti Zdroj/jímka objektů blob.

| Vlastnost aktivity kopírování | Vlastnost DataSet |
|:--- |:--- |
| skipHeaderLineCount na BlobSource |skipLineCount a firstRowAsHeader. Řádky jsou přeskočeny první a pak je pro čtení hello první řádek jako záhlaví. |
| treatEmptyAsNull na BlobSource |treatEmptyAsNull na vstupní datové sady |
| blobWriterAddHeader na BlobSink |firstRowAsHeader na výstupní datové sady |

V tématu [zadání TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) části Podrobné informace o těchto vlastností.    

### <a name="recursive-and-copybehavior-examples"></a>Příklady rekurzivní a copyBehavior
Tato část popisuje hello výsledné chování hello kopírování pro různé kombinace hodnot rekurzivní a copyBehavior.

| Rekurzivní | copyBehavior | Výsledné chování |
| --- | --- | --- |
| Hodnota TRUE |preserveHierarchy |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello stejné struktury jako zdroj hello<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| Hodnota TRUE |flattenHierarchy |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Vytvoření cíle Hello složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5 |
| Hodnota TRUE |mergeFiles |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Vytvoření cíle Hello složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor |
| False |preserveHierarchy |Pro zdrojové složky složku1 s hello strukturu: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello strukturu<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| False |flattenHierarchy |Pro zdrojové složky složku1 s hello strukturu:<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello strukturu<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/><br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| False |mergeFiles |Pro zdrojové složky složku1 s hello strukturu:<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hello cílové složce složku1 je vytvořena s hello strukturu<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor. automaticky generovaný název File1<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |

## <a name="walkthrough-use-copy-wizard-toocopy-data-tofrom-blob-storage"></a>Návod: Použití Průvodce kopírováním toocopy data do nebo z úložiště objektů Blob
Podíváme, jak tooquickly kopírování dat z Azure blob storage. V tomto návodu ukládá data zdrojového a cílového typu: Azure Blob Storage. Hello kanál v tomto návodu kopíruje data z tooanother složka v hello stejný kontejner objektů blob. Tento názorný postup je záměrně jednoduchá tooshow nastavení nebo vlastnosti při používání Blob Storage jako zdroj nebo jímky. 

### <a name="prerequisites"></a>Požadavky
1. Vytvoření pro obecné účely **účet úložiště Azure** Pokud již nemáte. Používání úložiště blob hello jako obě **zdroj** a **cílové** úložiště dat v tomto návodu. Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky toocreate jeden.
2. Vytvořte kontejner objektů blob s názvem **adfblobconnector** v účtu úložiště hello. 
4. Vytvořte složku s názvem **vstupní** v hello **adfblobconnector** kontejneru.
5. Vytvořte soubor s názvem **emp.txt** s hello následující obsahu a nahrajte ho toohello **vstupní** složky pomocí nástrojů, jako například [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-hello-data-factory"></a>Vytvoření objektu pro vytváření dat hello
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **+ nový** z levého horního rohu hello, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.
3. V hello **nový objekt pro vytváření dat** okno:   
    1. Zadejte **ADFBlobConnectorDF** pro hello **název**. Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se zobrazí chyba hello: `*Data factory name “ADFBlobConnectorDF” is not available`, změňte hello název objektu pro vytváření dat hello (například yournameADFBlobConnectorDF) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
    2. Vyberte své **předplatné** Azure.
    3. Pro skupinu prostředků, vyberte **použít existující** tooselect stávající prostředek skupiny (nebo) vyberte **vytvořit nový** tooenter název pro skupinu prostředků.
    4. Vyberte **umístění** hello data Factory.
    5. Vyberte **Pin toodashboard** políčko v hello dolní části okna hello.
    6. Klikněte na možnost **Vytvořit**.
3. Po dokončení vytvoření hello uvidíte hello **Data Factory** jak je uvedeno v následující obrázek hello: ![Domovská stránka objektu pro vytváření dat](./media/data-factory-azure-blob-connector/data-factory-home-page.png)

### <a name="copy-wizard"></a>Průvodce kopírováním
1. Na domovské stránce objektu pro vytváření dat hello, klikněte na tlačítko hello **kopírování dat [PREVIEW]** dlaždici toolaunch **Průvodce kopírováním dat** na samostatné kartě.    
    
    > [!NOTE]
    >    Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zakažte/zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality** nastavení (nebo) zachovat povolené a vytvořte výjimku pro **login.microsoftonline.com**a poté se pokuste spustit hello průvodce znovu.
2. V hello **vlastnosti** stránky:
    1. Zadejte **CopyPipeline** pro **název úlohy**. Název úlohy Hello je název hello hello kanálu v datové továrně.
    2. Zadejte **popis** pro úlohu hello (volitelné).
    3. Pro **cadence úloh nebo plán úloh**, zachovat hello **spuštění pravidelně podle plánu** možnost. Pokud chcete toorun tato úloha pouze jednou místo opakovaně spouštět podle plánu, vyberte **spustit jednou nyní**. Pokud vyberete, **spustit jednou nyní** možnost, [jednorázového kanálu](data-factory-create-pipelines.md#onetime-pipeline) je vytvořena. 
    4. Zachovat nastavení hello **opakovaná vzor**. Tato úloha spustí každý den mezi hello začínat a končit doby můžete zadat v dalším kroku hello.
    5. Změna hello **datum a čas zahájení** příliš**21/04/2017**. 
    6. Změna hello **datum a čas ukončení** příliš**04/25/2017**. Můžete chtít tootype hello datum místo projdete hello kalendáře.     
    8. Klikněte na **Další**.
      ![Nástroj pro kopírování – stránka vlastností](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png) 
3. Na hello **zdrojového úložiště dat** klikněte na tlačítko **Azure Blob Storage** dlaždici. Tato stránka toospecify hello zdrojového úložiště dat se používá pro úlohy kopie hello. Můžete použít existující propojenou službu úložiště dat nebo zadat nové úložiště dat. toouse existující propojená služba, byste měli vybrat **z existujících PROPOJENÝCH služeb** a hello vyberte požadovanou propojenou službu. 
    ![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)
4. Na hello **zadejte účet úložiště Azure Blob hello** stránky:
   1. Zachovat hello automaticky generovaný název pro **název připojení**. Název připojení Hello je název hello hello propojené služby typu: Azure Storage. 
   2. Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.
   3. Vyberte předplatné Azure nebo ponechte **Vybrat vše** pro **předplatné**.   
   4. Vyberte **účtu úložiště Azure** z hello účty k dispozici v rámci předplatného hello vybraný seznam úložiště Azure. Můžete také tooenter nastavení účtu úložiště ručně tak, že vyberete **zadat ručně** možnost pro hello **účet metodu výběru**.
   5. Klikněte na **Další**. 
      ![Nástroj pro kopírování – zadání účtu Azure Blob storage hello](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)
5. Na **zvolte hello vstupního souboru nebo složky** stránky:
   1. Klikněte dvakrát na **adfblobcontainer**.
   2. Vyberte **vstupní**a klikněte na tlačítko **zvolte**. V tomto návodu vyberte vstupní složky hello. Můžete třeba také vybrat hello emp.txt soubor ve složce hello místo. 
      ![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)
6. Na hello **zvolte hello vstupního souboru nebo složky** stránky:
    1. Potvrďte, že hello **souboru nebo složky** je nastaven příliš**adfblobconnector/vstup**. Pokud jsou soubory hello do podsložek, například 2017/04/01, 2017/04/02 a tak dále, zadání adfblobconnector / / {year} / {month} / {day} pro soubor nebo složku. Po stisknutí klávesy TAB mimo hello textového pole, uvidíte tři formáty tooselect rozevírací seznamy (rrrr) rok, měsíc (MM) a den (dd). 
    2. Nenastavujte **zkopírujte soubor rekurzivně**. Vyberte tuto možnost toorecursively křížovou prostřednictvím složek pro soubory toobe zkopírovaný toohello cíl. 
    3. Není hello **binární kopie** možnost. Vyberte tuto možnost tooperform binární kopie cíl toohello zdrojového souboru. Nevybírejte v tomto návodu tak, aby se zobrazí další možnosti v dalších stránkách hello. 
    4. Potvrďte, že hello **typ komprese** je nastaven příliš**žádné**. Hodnotu pro tuto možnost vyberte, pokud jsou v jednom z formátů hello podporované komprimované zdrojové soubory. 
    5. Klikněte na **Další**.
    ![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-azure-blob-connector/chose-input-file-folder.png) 
7. Na hello **nastavení formátu souboru** stránky, zobrazí hello oddělovače a hello schématu, která je automaticky nalezeny průvodcem hello analýzou soubor hello. 
    1. Potvrďte hello následující možnosti:. Hello **formát souboru** je nastaven příliš**formátu textu**. Můžete zobrazit všechny formáty hello podporované v rozevíracím seznamu hello. Příklad: formát JSON, Avro, ORC, Parquet.
        b. Hello **sloupec oddělovač** je nastaven příliš`Comma (,)`. Zobrazí se text hello další sloupec oddělovače v rozevíracím seznamu hello podporovány službou Data Factory. Můžete také zadat vlastní oddělovač.
        c. Hello **oddělovač řádků** je nastaven příliš`Carriage Return + Line feed (\r\n)`. Zobrazí se další řádek oddělovače podporovaných službou Data Factory v rozevíracím seznamu hello hello. Můžete také zadat vlastní oddělovač.
        d. Hello **Přeskočit počet řádků** je nastaven příliš**0**. Pokud chcete, aby přeskočil hello horní části souboru hello pár řádků toobe, zadejte zde číslo hello.
        e.  Hello **první řádek dat obsahuje názvy sloupců** není nastaven. Pokud hello zdrojové soubory obsahují názvy sloupců v hello první řádek, vyberte tuto možnost.
        f. Hello **považovat prázdný sloupec hodnota null** je možnost nastavena.
    2. Rozbalte položku **upřesňující nastavení** toosee rozšířené možnosti, které jsou k dispozici.
    3. V dolní části hello hello stránky, najdete v části hello **preview** dat ze souboru emp.txt hello.
    4. Klikněte na tlačítko **schématu** kartě v hello dolní toosee hello schématu tohoto Průvodce kopírováním hello odvodit prohlížením hello data hello zdrojového souboru.
    5. Klikněte na tlačítko **Další** po zkontrolujte hello oddělovače a zobrazte náhled dat.
    ![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)  
8. Na hello **úložiště dat cílové stránky**, vyberte **Azure Blob Storage**a klikněte na tlačítko **Další**. Hello Azure Blob Storage používají jako obou hello zdrojového a cílového úložiště dat v tomto návodu.    
    ![Nástroj pro kopírování – vyberte cílového úložiště dat](media/data-factory-azure-blob-connector/select-destination-data-store.png)
9. Na **zadejte účet úložiště Azure Blob hello** stránky:
   1. Zadejte **AzureStorageLinkedService** pro hello **název připojení** pole.
   2. Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.
   3. Vyberte své **předplatné** Azure.  
   4. Vyberte účet úložiště Azure. 
   5. Klikněte na **Další**.     
10. Na hello **zvolte hello výstupní soubor nebo složku** stránky: 
    6. Zadejte **cesta ke složce** jako **adfblobconnector výstupní / {year} / {month} / {day}**. Zadejte **KARTĚ**.
    7. Pro hello **roku**, vyberte **rrrr**.
    8. Pro hello **měsíc**, potvrďte, že je nastaven příliš**MM**.
    9. Pro hello **den**, potvrďte, že je nastaven příliš**dd**.
    10. Potvrďte, že hello **typ komprese** je nastaven příliš**žádné**.
    11. Potvrďte, že hello **zkopírujte chování** je nastaven příliš**sloučení souborů**. Pokud hello výstupní soubor s hello, které se stejným názvem již existuje, hello nový obsah je přidaný toohello stejný soubor na konci hello.
    12. Klikněte na **Další**.
    ![Nástroj pro kopírování – volba výstupního souboru nebo složky](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)
11. Na hello **nastavení formátu souboru** stránka, zkontrolujte hello nastavení a klikněte na tlačítko **Další**. Jedním z hello zde další možnosti je tooadd soubor hlaviček toohello výstup. Pokud tuto možnost vyberete, se přidá řádek záhlaví s názvy sloupců hello ze schématu hello hello zdroje. Při zobrazení hello schématu zdroje hello můžete přejmenovat hello výchozí názvy sloupců. Můžete například změnit hello první sloupec tooFirst název a druhý sloupec tooLast název. Potom hello výstupní soubor je vytvořen s záhlaví s těmito názvy jako názvy sloupců. 
    ![Nástroj pro kopírování – nastavení formátu souboru pro cíl](media/data-factory-azure-blob-connector/file-format-destination.png)
12. Na hello **nastavení výkonu** potvrďte, že **cloudu jednotky** a **paralelní kopie** nastaveny příliš**automaticky**a klikněte na tlačítko Další. Podrobnosti o těchto nastaveních najdete v tématu [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md#parallel-copy).
    ![Nástroj pro kopírování – nastavení výkonu](media/data-factory-azure-blob-connector/copy-performance-settings.png) 
14. Na hello **Souhrn** zkontrolujte všechna nastavení (Vlastnosti úlohy, nastavení pro zdrojové a cílové a Kopírovat nastavení) a klikněte na tlačítko **Další**.
    ![Nástroj pro kopírování – stránka souhrnu](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)
15. Zkontrolujte informace v hello **Souhrn** a klikněte na tlačítko **Dokončit**. Hello Průvodce vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál v objektu pro vytváření dat hello (z kde spouštěna hello Průvodce kopírováním).
    ![Nástroj pro kopírování – stránka nasazení](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)

### <a name="monitor-hello-pipeline-copy-task"></a>Monitorování kanálu hello (úlohy kopie)

1. Kliknutím na odkaz hello `Click here toomonitor copy pipeline` na hello **nasazení** stránky. 
2. Měli byste vidět hello **sledovat a spravovat aplikace** na samostatné kartě.  ![Sledování a správě aplikací](media/data-factory-azure-blob-connector/monitor-manage-app.png)
3. Změna hello **spustit** čas v horní části hello příliš`04/19/2017` a **end** čas příliš`04/27/2017`a potom klikněte na **použít**. 
4. Měli byste vidět pět oken aktivity v hello **aktivity WINDOWS** seznamu. Hello **WindowStart** časy by mělo zahrnovat všechny dny z kanálu počáteční toopipeline koncový čas. 
5. Klikněte na tlačítko **aktualizovat** tlačítko hello **aktivity WINDOWS** seznamu několikrát, dokud neuvidíte hello stav všechny aktivity windows hello je nastaven tooReady. 
6. Nyní ověřte, že jsou generovány hello výstupní soubory ve složce výstup hello adfblobconnector kontejneru. Měli byste vidět hello následující strukturu složek v hello výstupní složky: 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
Podrobné informace o monitorování a Správa objektů pro vytváření dat najdete v tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-app.md) článku. 
 
### <a name="data-factory-entities"></a>Entity objektu pro vytváření dat
Nyní přejděte zpět toohello karta s domovskou stránku hello Data Factory. Všimněte si, že existují dvě propojené služby, dvě datové sady a jeden kanál v datové továrně teď. 

![Domovská stránka objektu pro vytváření dat s entitami](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

Klikněte na tlačítko **vytvořit a nasadit** toolaunch editoru služby Data Factory. 

![Data Factory Editor](media/data-factory-azure-blob-connector/data-factory-editor.png)

Měli byste vidět následující entity služby Data Factory v datové továrně hello: 

 - Dvě propojené služby. Jednu pro hello zdroje a hello jiného pro cíl hello. Obě hello propojené služby odkazovat toohello stejný účet služby Azure Storage v tomto návodu. 
 - Dvě datové sady. Vstupní datové sady a datovou sadu výstupů. V tomto návodu, jak používat hello stejný kontejner objektů blob, ale odkazovat toodifferent složky (vstup a výstup).
 - Kanál. Hello kanál obsahuje aktivitu kopírování, který používá zdroj objektů blob a data toocopy podřízený objekt blob ze objektů blob v Azure umístění tooanother umístění objektu blob Azure. 

Hello následující části obsahují další informace o těchto entitách. 

#### <a name="linked-services"></a>Propojené služby
Měli byste vidět dvě propojené služby. Jednu pro hello zdroje a hello jiného pro cíl hello. V tomto návodu hello obou definice vzhled stejné s výjimkou hello názvy. Hello **typ** z hello propojené služby je nastaven příliš**azurestorage**. Nejdůležitější vlastnost definice hello propojené služby je hello **connectionString**, který je využíván jiným tooyour tooconnect objekt pro vytváření dat účet služby Azure Storage za běhu. Ignorujte hello hubName vlastnost v definici hello. 

##### <a name="source-blob-storage-linked-service"></a>Zdrojový objekt blob propojená služba úložiště
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a>Cílový objekt blob propojená služba úložiště

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

Další informace o propojené služby Azure Storage najdete v tématu [propojené vlastnosti služby](#linked-service-properties) části. 

#### <a name="datasets"></a>Datové sady
Existují dvě datové sady: vstupní datové sady a datovou sadu výstupů. Typ Hello sady dat hello je nastaven příliš**AzureBlob** pro obojí. 

vstupní datové sady Hello body toohello **vstupní** složky hello **adfblobconnector** kontejner objektů blob. Hello **externí** vlastnost je nastavena příliš**true** pro tuto datovou sadu jako hello není data vytvořená hello kanálu s aktivitou kopírování hello, která přebírá tuto datovou sadu jako vstup. 

Hello výstupní datovou sadu body toohello **výstup** složky hello stejný kontejner objektů blob. Hello výstupní datové sady používá také hello rok, měsíc a den hello **SliceStart** proměnné toodynamically systému vyhodnotit hello cesta k souboru výstup hello. Seznam funkcí a systémové proměnné podporovaných službou Data Factory najdete v tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md). Hello **externí** vlastnost je nastavena příliš**false** (výchozí hodnota) protože tato datová sada je produkovaný hello kanálu. 

Další informace o vlastnostech podporovaných zprostředkovatelem datovou sadu objektu Blob Azure najdete v tématu [vlastnosti datové sady](#dataset-properties) části.

##### <a name="input-dataset"></a>Vstupní datové sady

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a>Výstupní datové sady

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a>Kanál
Hello kanálu má jenom jednu aktivitu. Hello **typ** Dobrý den aktivity je nastavený příliš**kopie**.  Ve vlastnostech typu hello aktivity hello jsou dvě části, jednu pro zdroje a hello jiného pro sink. Typ zdroje Hello je nastaven příliš**BlobSource** jako aktivita hello je kopírování dat z úložiště objektů blob. Hello typ jímky je nastaven příliš**BlobSink** jako hello aktivita kopírování úložiště objektů blob tooa data. Aktivita kopírování Hello vezme jako vstupní hello a OutputDataset z4y jako výstup hello InputDataset z4y. 

Další informace o vlastnostech podporovaných zprostředkovatelem BlobSource a BlobSink najdete v tématu [zkopírovat vlastnosti aktivity](#copy-activity-properties) části. 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-blob-storage"></a>Příklady JSON pro kopírování tooand dat z úložiště objektů Blob  
Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ukazují jak toocopy tooand dat z Azure Blob Storage a Azure SQL Database. Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.

### <a name="json-example-copy-data-from-blob-storage-toosql-database"></a>Příklad JSON: Kopírování dat z úložiště objektů Blob tooSQL databáze
Následující ukázka ukazuje Hello:

1. Propojené služby typu [azuresqldatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Propojené služby typu [azurestorage](#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](#copy-activity-properties) a [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).

Ukázka Hello kopíruje data časové řady z tabulky Azure SQL tooan objektů blob v Azure každou hodinu. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Azure SQL propojené služby:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Propojená služba Azure Storage:**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**. Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS). V tématu [propojené služby](#linked-service-properties) podrobnosti.  

**Azure vstupní datovou sadu objektu Blob:**

Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1). název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas. "externí": "PRAVDA" nastavení informuje objekt pro vytváření dat, tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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
**Azure SQL výstupní datovou sadu:**

Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v databázi Azure SQL. Vytvoření hello tabulky v databázi Azure SQL s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello. Přidávání řádků tabulky toohello každou hodinu.

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Aktivita kopírování v kanálu s Blob zdroj a jímka SQL:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
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
### <a name="json-example-copy-data-from-azure-sql-tooazure-blob"></a>Příklad JSON: Kopírování dat z Azure SQL tooAzure objektů Blob
Následující ukázka ukazuje Hello:

1. Propojené služby typu [azuresqldatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Propojené služby typu [azurestorage](#linked-service-properties).
3. Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
4. Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](#dataset-properties).
5. A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) a [BlobSink](#copy-activity-properties).

Ukázka Hello kopíruje data časové řady každou hodinu z tooan tabulky Azure SQL Azure blob. Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.

**Azure SQL propojené služby:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Propojená služba Azure Storage:**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**. Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS). V tématu [propojené služby](#linked-service-properties) podrobnosti.  

**Azure SQL vstupní datové sady:**

Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Azure SQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.

Nastavení "externí": "PRAVDA" informuje služba Data Factory Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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

**Azure Blob výstupní datovou sadu:**

Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1). Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány. Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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

**Aktivita kopírování v kanálu s SQL zdrojový a podřízený objekt Blob:**

Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**BlobSink**. Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
