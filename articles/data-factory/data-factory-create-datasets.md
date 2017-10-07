---
title: "datové sady aaaCreate v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak toocreate datové sady v Azure Data Factory s příklady, které používají vlastnosti, jako posun a anchorDateTime."
keywords: "Vytvoření datové sady, například datové sady, posun příklad"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: shlo
ms.openlocfilehash: 181859ed250595d756df73e9ebcac08d9e7184c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="datasets-in-azure-data-factory"></a>Datové sady ve službě Azure Data Factory
Tento článek popisuje, jaké datové sady se, jak jsou definovány ve formátu JSON, a způsobu jejich použití v Azure Data Factory kanálů. Poskytuje podrobnosti o jednotlivých částech (například struktura, dostupnost a zásad) v definici JSON hello datové sady. Hello článku také obsahuje příklady použití hello **posun**, **anchorDateTime**, a **styl** vlastnosti v definici JSON datové sady.

> [!NOTE]
> Pokud jste tooData nový objekt pro vytváření, najdete v části [Úvod tooAzure Data Factory](data-factory-introduction.md) Přehled. Pokud nemáte praktických zkušeností s vytvářením objektů pro vytváření dat, vám může pomoci lépe porozumět ve čtení hello [kurzu transformaci dat](data-factory-build-your-first-pipeline.md) a hello [kurzu přesun dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="overview"></a>Přehled
Objekt pro vytváření dat může mít jeden nebo víc kanálů. A **kanálu** je logické seskupení **aktivity** společně provádějí úlohy. Hello aktivity v kanálu definují akce tooperform na vaše data. Můžete například použít kopie aktivity toocopy data z tooAzure systému SQL Server místní úložiště objektů Blob. Pak můžete použít aktivitu Hive, která se spouští skript Hive na model data tooprocess clusteru Azure HDInsight z objektu Blob úložiště tooproduce výstupní data. Nakonec můžete použít druhé kopie aktivity toocopy hello výstupní data tooAzure SQL Data Warehouse, nad kterou business intelligence (BI), vytváření sestav jsou integrované řešení. Další informace o kanály a aktivity, najdete v části [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md).

Aktivita může trvat vstup nula nebo více **datové sady**a vytvoří výstupní datové sady. Vstupní datové sady reprezentuje hello vstup pro aktivitu v kanálu hello a výstupní datové reprezentuje hello výstup aktivity hello. Datové sady identifikují data v rámci různých úložišť dat, jako jsou tabulky, soubory, složky a dokumenty. Například datové sadě služby Azure Blob určuje hello kontejner objektů blob a složky v úložišti objektů Blob, ze které hello kanálu by měl číst hello data. 

Než vytvoříte datové sady, vytvořit **propojená služba** toolink data ukládat toohello data factory. Propojené služby jsou mnohem jako připojovací řetězce, které definují hello připojení informace potřebné pro vytváření dat tooconnect tooexternal prostředky. Datové sady identifikují dat v rámci hello propojená datová úložiště, jako jsou tabulky SQL, souborů, složek a dokumentů. Například Azure Storage propojená služba propojuje vytváření dat toohello účet úložiště. Datové sadě služby Azure Blob představuje kontejner objektů blob hello a hello složku, která obsahuje toobe vstupní objekty BLOB hello zpracovat. 

Tady je ukázkový scénář. toocopy data z databáze SQL tooa úložiště objektů Blob, vytvoříte dvě propojené služby: úložiště Azure a Azure SQL Database. Pak vytvořte dvě datové sady: datovou sadu objektu Blob Azure, (který se týká toohello propojené služby Azure Storage) a tabulky SQL Azure datovou sadu (odkazuje toohello Azure SQL Database propojené služby). Hello Azure Storage a Azure SQL Database propojené služby obsahovat připojovací řetězce, které objekt pro vytváření dat používá v modulu runtime tooconnect tooyour Azure Storage a Azure SQL Database, v uvedeném pořadí. datovou sadu objektu Blob Azure Hello určuje hello kontejner objektů blob a objektů blob složku, která obsahuje hello vstupní objekty BLOB ve službě Blob storage. Datová sada tabulky SQL Azure Hello Určuje, že hello tabulky SQL ve vašich datech hello toowhich databáze SQL je toobe zkopírovali.

Hello následující obrázek znázorňuje vztahy hello mezi kanálu, aktivity, datové sady a propojené služby objektu pro vytváření dat: 

![Vztah mezi kanálu, aktivity, datové sady, propojených služeb](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>JSON datové sady
Datové sady ve službě Data Factory je definováno ve formátu JSON následujícím způsobem:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

Hello následující tabulka popisuje vlastnosti v hello výše JSON:   

| Vlastnost | Popis | Požaduje se | Výchozí |
| --- | --- | --- | --- |
| jméno |Název datové sady hello. V tématu [Azure Data Factory - pravidla po pojmenování](data-factory-naming-rules.md) pravidla pojmenování. |Ano |Není k dispozici |
| type |Typ hello datovou sadu. Zadejte jeden z typů hello podporovaných službou Data Factory (například: AzureBlob, AzureSqlTable). <br/><br/>Podrobnosti najdete v tématu [typ sady](#Type). |Ano |Není k dispozici |
| Struktura |Schéma hello datovou sadu.<br/><br/>Podrobnosti najdete v tématu [strukturu datové sady](#Structure). |Ne |Není k dispozici |
| typeProperties | vlastnosti typu Hello se liší pro jednotlivé typy (například: Azure Blob, tabulka Azure SQL). Podrobnosti o svých vlastnostech a hello podporované typy najdete v tématu [typ sady](#Type). |Ano |Není k dispozici |
| external | Logická hodnota příznak toospecify, zda datové sady je explicitně produkovaný kanálu objekt pro vytváření dat nebo ne. Pokud není hello vstupní datové sady pro aktivitu vytvořil hello aktuální kanálu, nastavte tento příznak tootrue. Nastavte tento příznak tootrue pro hello vstupní datové sady hello první aktivitu v kanálu hello.  |Ne |False |
| dostupnosti | Definuje hello okno zpracování (například hodinové nebo denní) nebo hello řezů model pro produkční hello datovou sadu. Jednotlivé jednotky data využívat a vyprodukované spuštění aktivity se nazývá datový řez. Pokud se hello dostupnost výstupní datové sady toodaily (frekvenci - den, interval - 1), řez vytváří denně. <br/><br/>Podrobnosti najdete v tématu [datovou sadu dostupnosti](#Availability). <br/><br/>Podrobnosti o datovou sadu hello řezů modelu, najdete v části hello [plánování a provádění](data-factory-scheduling-and-execution.md) článku. |Ano |Není k dispozici |
| policy |Definuje kritéria hello nebo hello podmínku, která musíte splnit řezy hello datovou sadu. <br/><br/>Podrobnosti najdete v tématu hello [datovou sadu zásad](#Policy) části. |Ne |Není k dispozici |

## <a name="dataset-example"></a>Příklad datové sady
V následující ukázka hello, datová sada hello představuje tabulku s názvem **MyTable** v databázi SQL.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Všimněte si hello následující body:

* **typ** nastavena tooAzureSqlTable.
* **Název tabulky** vlastnost type (typ konkrétní tooAzureSqlTable) je nastavená tooMyTable.
* **linkedServiceName** odkazuje tooa propojené služby typu azuresqldatabase., která je definována v hello další fragmentu kódu JSON. 
* **frekvence dostupnosti** nastavena tooDay, a **interval** nastavena too1. To znamená, že hello datovou sadu se vytvářejí denně.  

**AzureSqlLinkedService** je definován následujícím způsobem:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

V předchozím fragmentu kódu JSON hello:

* **typ** nastavena tooAzureSqlDatabase.
* **connectionString** vlastnost typu Určuje informace tooconnect tooa SQL database.  

Jak vidíte, hello propojené služby definuje jak tooconnect tooa SQL database. Datová sada Hello definuje, jaký tabulka je použít jako vstup a výstup hello aktivity v kanálu.   

> [!IMPORTANT]
> Pokud datové sady je vytvářen hello kanálu, by měl být označen jako **externí**. Toto nastavení obecně platí tooinputs první aktivitu v kanálu.   


## <a name="Type"></a>Typ sady
Typ Hello hello sady dat závisí na hello úložiště dat, které používáte. Viz následující tabulka obsahuje seznam podporovaných službou Data Factory úložiště dat hello. Klikněte na tlačítko data store toolearn jak toocreate propojené služby a datové sady pro tato data uložit.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Úložiště dat, s * může být místní nebo v Azure infrastruktura jako služba (IaaS). Tyto úložiště dat vyžadují tooinstall [Brána pro správu dat](data-factory-data-management-gateway.md).

V příkladu hello v předchozí části hello hello typ DataSet hello je nastaven příliš**AzureSqlTable**. Podobně pro datové sadě služby Azure Blob hello hello sady dat je typ nastaven příliš**AzureBlob**, jak je znázorněno v následujícím JSON hello:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

## <a name="Structure"></a>Struktura datové sady
Hello **struktura** část je nepovinná. Definuje schéma hello datovou sadu hello podle obsahující kolekci názvů a typy dat sloupců. Použijete hello struktura části tooprovide typ informace, které jsou používané tooconvert typy a mapování sloupců z hello zdroj toohello cíl. V následující ukázka hello, hello datová sada má tři sloupce: `slicetimestamp`, `projectname`, a `pageviews`. Jsou typu řetězec, řetězec a Decimal, v uvedeném pořadí.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Každý sloupec struktury hello obsahuje hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název sloupce hello. |Ano |
| type |Datový typ sloupce hello.  |Ne |
| Jazyková verze |. Jazykovou verzi na základě NET toobe používá při hello typ je typ formátu .NET: `Datetime` nebo `Datetimeoffset`. Výchozí hodnota Hello je `en-us`. |Ne |
| Formát |Formátování toobe řetězec se používá při hello typ je typ formátu .NET: `Datetime` nebo `Datetimeoffset`. |Ne |

Hello následující pokyny vám pomohou určit, kdy tooinclude struktury informace a jaké tooinclude v hello **struktura** části.

* **Pro strukturovaná data zdroje**, zadejte část struktura hello pouze v případě, že chcete, aby mapování zdrojových sloupců toosink sloupců a jejich názvy nejsou hello stejné. Tento druh zdroj strukturovaných dat ukládá informace schématu a typu dat společně s vlastními daty hello. Příklady strukturovaných dat zdroje: SQL Server, Oracle a tabulky Azure. 
  
    Protože je již k dispozici pro strukturovaná data zdroje informací o typu, by neměla zahrnovat informace o typu, obsahují části struktura hello.
* **Pro schéma pro zdroje dat pro čtení (konkrétně úložiště objektů Blob)**, můžete toostore data bez ukládání žádné schéma nebo typ informace s daty hello. Pro tyto typy zdrojů dat zahrnují struktura, když chcete, aby toomap zdrojové sloupce toosink sloupce. Také zahrnovat struktura hello datová sada vstupem pro aktivitu kopírování a datové typy datovou sadu zdroj by měl být převedená toonative typy pro hello sink. 
    
    Objekt pro vytváření dat podporuje následující hodnoty pro poskytnutí informací o typu ve struktuře hello: **Int16, Int32, Int64, jedním, Double, Decimal, Byte [], logická hodnota, řetězec, Guid, Datetime, Datetimeoffset a časový interval**. Tyto hodnoty jsou specifikace CLS (Common Language)-vyhovující,. Na základě NET typ hodnoty.

Objekt pro vytváření dat převody typů automaticky provede při přesunu, že data ze zdrojových dat úložiště tooa podřízený data. 
  

## <a name="dataset-availability"></a>Datovou sadu dostupnosti
Hello **dostupnosti** oddíl v datové sadě definuje hello okna pro zpracování (například hodinový, denní, nebo každý týden) pro datovou sadu hello. Další informace o aktivity windows najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md).

Následující části dostupnosti Hello Určuje, že hello výstupní datovou sadu se buď vytvářejí hodinu nebo vstupní datové sady hello každou hodinu je k dispozici:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Když hello kanálu hello následující počáteční a koncový čas:  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

Hello výstupní datovou sadu se vytvářejí každou hodinu v rámci kanálu hello spuštění a ukončení. Proto jsou pět řezy datové sady vyprodukované tímto kanálem, jeden pro každou aktivitu okno (12: 00 - 1 AM, 1: 00 - 2 AM, 2: 00 - 3 AM, 3: 00 - 4 AM, 4: 00 - 5: 00). 

Hello následující tabulka popisuje vlastnosti, které můžete použít v části hello dostupnosti:

| Vlastnost | Popis | Požaduje se | Výchozí |
| --- | --- | --- | --- |
| frequency |Určuje časovou jednotku hello k produkci řez datovou sadu.<br/><br/><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc |Ano |Není k dispozici |
| interval |Určuje multiplikátor pro četnost.<br/><br/>"Frekvence x interval" Určuje, jak často hello se vytvářejí. Například pokud potřebují hello datovou sadu toobe rozříznut hodinu, nastavíte <b>frekvence</b> příliš<b>hodinu</b>, a <b>interval</b> příliš<b>1</b>.<br/><br/>Všimněte si, že pokud zadáte **frekvence** jako **minutu**, byste měli nastavit hello interval toono méně než 15. |Ano |Není k dispozici |
| Styl |Určuje, zda by měl být na hello počáteční nebo koncový intervalu hello předložen hello řez.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul>Pokud **frekvence** je nastaven příliš**měsíc**, a **styl** je nastaven příliš**EndOfInterval**, hello se vytvářejí na hello poslední den v měsíci. Pokud **styl** je nastaven příliš**StartOfInterval**, hello se vytvářejí na hello první den v měsíci.<br/><br/>Pokud **frekvence** je nastaven příliš**den**, a **styl** je nastaven příliš**EndOfInterval**, hello se vytvářejí v hello poslední hodiny dne hello.<br/><br/>Pokud **frekvence** je nastaven příliš**hodinu**, a **styl** je nastaven příliš**EndOfInterval**, hello se vytvářejí na konci hello hello hodina. Například pro řez pro hello období 13: 00 – 14: 00, hello se vytvářejí na 14: 00. |Ne |EndOfInterval |
| anchorDateTime |Definuje hello absolutní pozici v čase, které používá hello Plánovač toocompute datovou sadu řez hranice. <br/><br/>Všimněte si, že pokud má tento propoerty částí data, která jsou podrobnější než hello zadané frekvence hello podrobnější částí se ignorují. Například, pokud hello **interval** je **každou hodinu** (frekvence: hodin a interval: 1) a hello **anchorDateTime** obsahuje **minuty a sekundy**, pak hello minut a sekund **anchorDateTime** jsou ignorovány. |Ne |01/01/0001 |
| Posun |Časový interval, ve které hello začátku a konci všech řezech datovou sadu posunuty. <br/><br/>Všimněte si, že pokud obě **anchorDateTime** a **posun** jsou nastaveny, výsledkem hello je hello kombinaci shift. |Ne |Není k dispozici |

### <a name="offset-example"></a>Příklad posunutí
Ve výchozím nastavení, každý den (`"frequency": "Day", "interval": 1`) řezy začnou ve 12: 00 (půlnoc) koordinovaný světový čas (UTC). Pokud chcete místo toho hello počáteční čas čas UTC toobe 6: 00, nastavte hello posun, jak je znázorněno v následujícím fragmentu kódu hello: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>Příklad anchorDateTime
V následujícím příkladu hello hello sada vytváří jednou za 23 hodin. Hello první řez spustí v době hello určeného **anchorDateTime**, který je nastaven příliš`2017-04-19T08:00:00` (UTC).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>Posun nebo styl příklad
Hello následující datová sada každý měsíc a vytváří hello 3rd v každém měsíci v 8:00 AM (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>Datovou sadu zásad
Hello **zásad** oddíl v definici datové sady hello definuje kritéria hello nebo hello podmínku, která hello řezy datovou sadu musí splnit.

### <a name="validation-policies"></a>Zásady ověřování
| Název zásady | Popis | Použít příliš| Požaduje se | Výchozí |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Ověří, že hello data v **úložiště objektů Azure Blob** hello splňuje požadavky na minimální velikost (v megabajtech). |Azure Blob Storage |Ne |Není k dispozici |
| minimumRows |Ověří, zda hello data ve **Azure SQL database** nebo **tabulky Azure** obsahuje hello minimální počet řádků. |<ul><li>Databáze SQL Azure</li><li>Tabulky Azure</li></ul> |Ne |Není k dispozici |

#### <a name="examples"></a>Příklady
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows:**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a>Externích datových sad
Externích datových sad jsou ty, které nejsou od spuštěné kanál v objektu pro vytváření dat hello hello. Pokud hello datové sady je označena jako **externí**, hello **ExternalData** zásad může být definovaná tooinfluence hello chování hello datovou sadu řez dostupnosti.

Pokud datové sady je vytvářen službou Data Factory, by měl být označen jako **externí**. Toto nastavení obecně platí toohello vstupy první aktivitu v kanálu, pokud se používá aktivitu nebo řetězení kanálu.

| Name (Název) | Popis | Požaduje se | Výchozí hodnota |
| --- | --- | --- | --- |
| dataDelay |čas Hello toodelay hello zkontrolovat dostupnost hello hello externích dat pro hello daného řez. Můžete například zpoždění hodinové kontroly pomocí tohoto nastavení.<br/><br/>Hello nastavení pouze platí toohello aktuální čas.  Například pokud je 1:00 PM hned teď a tato hodnota je 10 minut, ověření hello se spustí: 10: 00.<br/><br/>Všimněte si, že toto nastavení neovlivňuje řezy v posledních hello. Řezy s **řez koncový čas** + **dataDelay** < **nyní** jsou zpracovávány bez jakéhokoli zpoždění.<br/><br/>Časy větší než 23:59 hodin zadat pomocí hello `day.hours:minutes:seconds` formátu. Například toospecify 24 hodin, nepoužívejte 24:00:00. Místo toho použijte 1.00:00:00. Pokud používáte 24:00:00, bude považován za 24 dní (24.00:00:00). 1 den a 4 hodiny zadejte 1:04:00:00. |Ne |0 |
| RetryInterval |Doba čekání Hello mezi selhání i hello další pokusy. Toto nastavení platí toopresent čas. Pokud se nezdařila předchozí zkuste hello, zkuste další hello po hello **retryInterval** období. <br/><br/>Pokud je 1:00 PM nyní, můžeme začít hello prvního pokusu. Pokud hello trvání toocomplete hello první ověření kontrola je 1 minuta a hello operace se nezdařila, hello další pokus proběhne v 1:00 + 1 min (doba trvání) + 1min (interval opakování) = 1:02 PM. <br/><br/>Řezy v posledních hello neexistuje žádné zpoždění není. Hello opakování dojde okamžitě. |Ne |00:01:00 (1 min) |
| retryTimeout |Hello časový limit pro jednotlivé pokusy o opakování.<br/><br/>Pokud je tato vlastnost nastavená too10 minut hello ověření musí být dokončeny v rámci 10 minut. Pokud trvá déle než 10 minut tooperform hello ověření, opakujte hello časový limit.<br/><br/>Pokud všechny pokusy o hello ověření časového limitu hello řez je označen jako **TimedOut**. |Ne |00:10:00 (10 minut) |
| maximumRetry |Hello kolikrát toocheck hello dostupnost externích dat hello. Hello maximální povolená hodnota je 10. |Ne |3 |


## <a name="create-datasets"></a>Vytvoření datových sad
Datové sady můžete vytvořit pomocí jedné z těchto nástrojů nebo sady SDK: 

- Průvodce kopírováním 
- portál Azure
- Visual Studio
- PowerShell
- Šablona Azure Resource Manageru
- REST API
- .NET API

V tématu hello následující kurzy pro podrobné pokyny pro vytváření kanálů a datové sady pomocí jedné z těchto nástrojů nebo sady SDK:
 
- [Vytvoření kanálu s aktivitou transformace dat](data-factory-build-your-first-pipeline.md)
- [Vytvoření kanálu s aktivitou přesun dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Po kanálu se vytvoří a nasadí, můžete spravovat a monitorovat hello kanály pomocí oken webu Azure portal nebo hello monitorování a správu aplikace. V tématu hello následující témata podrobné pokyny: 

- [Monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md)
- [Monitorování a Správa kanálů pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a>Oboru datové sady
Můžete vytvořit datové sady, které jsou vymezená tooa kanálu pomocí hello **datové sady** vlastnost. Tyto datové sady lze použít pouze aktivity v rámci tohoto kanálu, nikoli aktivity v jiné kanály. Následující ukázka Hello definuje kanál s dvě datové sady (InputDataset rdc a OutputDataset rdc) toobe používaných v rámci kanálu hello.  

> [!IMPORTANT]
> Oboru datové sady jsou podporovány pouze s jednorázové kanály (kde **pipelineMode** je nastaven příliš**OneTime**). V tématu [Onetime kanálu](data-factory-create-pipelines.md#onetime-pipeline) podrobnosti.
>
>

```json
{
    "name": "CopyPipeline-rdc",
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
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
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
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Další kroky
- Další informace o kanály najdete v tématu [vytvořit kanály](data-factory-create-pipelines.md). 
- Další informace o tom, jak jsou naplánované kanálů a jsou prováděny najdete v tématu [plánování a provádění v Azure Data Factory](data-factory-scheduling-and-execution.md). 
