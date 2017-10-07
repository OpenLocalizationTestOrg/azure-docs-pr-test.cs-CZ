---
title: "aaaAzure objekt pro vytváření dat – referenční dokumentace skriptování JSON | Microsoft Docs"
description: "Poskytuje schémata JSON entit služby Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 813fd752bb0ecb1b513d022b9f302325105dac31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a>Azure Data Factory - referenčních informacích o skriptování JSON
Tento článek obsahuje schémata JSON a příklady pro definování entity Azure Data Factory (kanál, aktivity, datové sady a propojené služby).  

## <a name="pipeline"></a>Kanál 
Hello základní strukturu pro definici kanálu je následující: 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Následující tabulka popisuje vlastnosti hello v rámci kanálu hello definici JSON:

| Vlastnost | Popis | Požaduje se
-------- | ----------- | --------
| jméno | Název kanálu hello. Zadejte název, který představuje hello akci, která hello aktivity nebo kanál je nakonfigurované toodo<br/><ul><li>Maximální počet znaků: 260.</li><li>Musí začínat písmenem, číslicí nebo podtržítkem (_).</li><li>Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Ano |
| description |Popisuje, jaké aktivity hello nebo kanálu se používá pro text | Ne |
| activities | Obsahuje seznam aktivit. | Ano |
| start |Počáteční datum a čas pro kanál hello. Musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Příklad: 2014-10-14T16:32:41. <br/><br/>Je možné toospecify místního času, například Odhadovaný čas. Tady je příklad: `2016-02-27T06:00:00**-05:00`, což je odhadované AM 6<br/><br/>Hello počáteční a koncové vlastnosti společně zadejte aktivní období pro kanál hello. Výstup řezy jenom vytváří se v tomto aktivní období. |Ne<br/><br/>Pokud zadáte hodnotu pro vlastnost end hello, musíte zadat hodnotu pro vlastnost začátku hello.<br/><br/>Hello počáteční a koncový čas může být prázdný toocreate kanálu. Je potřeba zadat obě hodnoty tooset na aktivní období kanálu toorun hello. Pokud nezadáte počáteční a koncový čas při vytváření kanálu, můžete je nastavit pomocí rutiny Set-AzureRmDataFactoryPipelineActivePeriod hello později. |
| End |Koncové datum a čas pro kanál hello. Pokud zadaný, musí být ve formátu ISO. Příklad: 2014-10-14T17:32:41 <br/><br/>Je možné toospecify místního času, například Odhadovaný čas. Tady je příklad: `2016-02-27T06:00:00**-05:00`, což je odhadované AM 6<br/><br/>9999-09-09 toorun hello kanálu bez omezení, zadejte jako hello hodnotu pro vlastnost end hello. |Ne <br/><br/>Pokud zadáte hodnotu pro vlastnost začátku hello, musíte zadat hodnotu pro vlastnost end hello.<br/><br/>Naleznete v poznámkách k hello **spustit** vlastnost. |
| isPaused |Pokud sada tootrue hello kanálu nelze spustit. Výchozí hodnota = false. Můžete použít tento tooenable vlastnosti nebo zakázat. |Ne |
| pipelineMode |Metoda Hello plánování spuštění pro hello kanálu. Povolené hodnoty jsou: naplánované (výchozí), jednorázově.<br/><br/>"Pravidelnou" označuje, že kanál hello se spustí v zadaném časovém intervalu podle tooits aktivní období (počáteční a koncový čas). "Jednorázově" označuje, že kanál hello spustí jenom jednou. Po vytvoření jednorázově kanály nelze aktuálně upravit nebo aktualizovat. V tématu [Onetime kanálu](data-factory-create-pipelines.md#onetime-pipeline) podrobnosti o jednorázově nastavení. |Ne |
| ExpirationTime |Doba, po vytvoření, pro které hello kanálu je platný a by měla zůstat zřízené. Pokud nemá žádné aktivní, se nezdařilo, nebo čekající spuštění kanálu hello automaticky odstraněna po nedosáhne hello čas vypršení platnosti. |Ne |


## <a name="activity"></a>Aktivita 
Základní struktura Hello pro aktivitu v rámci kanálu definice (aktivity element) je následující:

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    }
    "scheduler":
    {
    }
}
```

Následující tabulky popisují hello vlastnosti v rámci aktivity hello definici JSON:

| Značka | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název aktivity hello. Zadejte název, který představuje hello akci, která aktivita hello nakonfigurovat toodo<br/><ul><li>Maximální počet znaků: 260.</li><li>Musí začínat písmenem, číslicí nebo podtržítkem (_).</li><li>Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Ano |
| description |Popisuje, jaké aktivity hello se používá pro text. |Ano |
| type |Určuje typ hello hello aktivity. V tématu hello [ÚLOŽIŠŤ dat](#data-stores) a [aktivit TRANSFORMACE dat](#data-transformation-activities) oddíly pro různé typy aktivit. |Ano |
| Vstupy |Vstupní tabulky použité aktivitou hello<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Ano |
| Výstupy |Výstupní tabulky použité aktivitou hello.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Ano |
| linkedServiceName |Název hello propojené služby používané hello aktivity. <br/><br/>Aktivita může vyžadovat, že zadáváte hello propojené služby, která propojí toohello požadované výpočetním prostředí. |Ano pro aktivity HDInsight, Azure Machine Learning aktivity a aktivity uložené procedury. <br/><br/>Ne ve všech ostatních případech |
| typeProperties |Vlastnosti v rámci typeProperties části hello závisí na typu aktivity hello. |Ne |
| policy |Zásady, které ovlivňují chování běhové hello hello aktivity. Pokud není zadaný, použijí se výchozí zásady. |Ne |
| Scheduler |Vlastnost "scheduler" je použité toodefine potřeby plánování aktivity hello. Jeho podvlastnosti jsou hello stejné jako ty, které v hello hello [vlastnost availability v datové sadě](data-factory-create-datasets.md#dataset-availability). |Ne |

### <a name="policies"></a>Zásady
Zásady ovlivňují chování hello běhu aktivity, konkrétně v případě, že je zpracování řezu hello tabulky. Hello následující tabulka poskytuje podrobnosti hello.

| Vlastnost | Povolené hodnoty | Výchozí hodnota | Popis |
| --- | --- | --- | --- |
| Souběžnosti |Integer <br/><br/>Maximální hodnota: 10 |1 |Počet souběžných spuštění aktivity hello.<br/><br/>Určuje hello počet spuštěních paralelní aktivity, které se může stát při jiné řezy. Například pokud aktivita vyžaduje toogo prostřednictvím velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat hello. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Určuje pořadí hello datové řezy, které jsou zpracovávány.<br/><br/>Pokud máte 2 řezy (jeden situaci ve 4 a další v 17: 00) a jsou obě čekající na zpracování. Pokud jste nastavili hello executionPriorityOrder toobe NewestFirst, hello řez v 17: 00, je zpracován jako první. Podobně pokud nastavíte hello executionPriorityORder toobe OldestFIrst, pak hello ve 4 zpracování řezu se. |
| retry |Integer<br/><br/>Maximální hodnota může být 10 |0 |Počet opakovaných pokusů před hello zpracování dat pro hello řez je označena jako selhání. Provedení aktivity pro datový řez je opakovat až toohello zadaný počet opakování. co nejdříve po selhání hello se provádí Hello opakování. |
| timeout |Časový interval |00:00:00 |Časový limit aktivity hello. Příklad: 00:10:00 (znamená časový limit 10 minut)<br/><br/>Pokud hodnota není zadána nebo je 0, vypršení časového limitu hello je nekonečno.<br/><br/>Pokud doba zpracování dat hello na řez překročí hodnota časového limitu hello, se zruší a hello systém pokusí tooretry hello zpracování. Hello počet opakovaných pokusů závisí na vlastnosti opakování hello. Když dojde k vypršení časového limitu, je nastaven stav hello tooTimedOut. |
| Zpoždění |Časový interval |00:00:00 |Zadejte zpoždění hello před spuštěním zpracování dat hello řez.<br/><br/>Hello provádění aktivity pro datový řez je spuštěn v minulosti hello očekávaný čas provádění po hello zpoždění.<br/><br/>Příklad: 00:10:00 (znamená zpoždění 10 minut) |
| opakování po delší době |Integer<br/><br/>Maximální hodnota: 10 |1 |Hello počet dlouho opakování pokusů, než hello řez spuštění se nezdařilo.<br/><br/>pokusy o opakování po delší době jsou rozmístěny ve longRetryInterval. Takže pokud budete potřebovat toospecify doba mezi pokusy o opakování, použijte opakování po delší době. Pokud jsou zadané opakování a opakování po delší době, jednotlivé pokusy o opakování po delší době zahrnuje opakovaných pokusů a je hello maximální počet pokusů o opakování * opakování po delší době.<br/><br/>Například, pokud bychom měli hello následující nastavení v zásadě hello aktivity:<br/>Opakujte: 3<br/>opakování po delší době: 2<br/>longRetryInterval: 01:00:00<br/><br/>Předpokládá se jenom jeden řez tooexecute (stav Čeká) a provedení aktivity hello pokaždé, když dojde k chybě. Nejdřív by 3 provádění po sobě jdoucích pokusů. Po každém pokusu o stav řezu hello bude opakovat. Po první 3 pokusy jsou přes, bude stav řezu hello opakování po delší době.<br/><br/>Po hodině (který je na longRetryInteval hodnota) bude další sadu 3 provádění po sobě jdoucích pokusů. Od tohoto stavu řezu hello by se nezdařilo a by se pokus o žádné další opakování. Proto celkové 6 pokusy byly provedeny.<br/><br/>Pokud žádné spuštění úspěšné, stav řezu hello by připravené a jsou pokus o žádné další opakování.<br/><br/>opakování po delší době je možné použít situace, kdy závislé data dorazí v časech Nedeterministický nebo hello celém prostředí je v nestabilním stavu v rámci které zpracování dat dojde. V takových případech nemusí být úspěšná při provádění opakování, jedna po druhé a tak v intervalech čas má za následek hello potřeby výstup.<br/><br/>Word varování: nenastavujte vysoké hodnoty pro opakování po delší době nebo longRetryInterval. Vyšší hodnoty obvykle implikují dalších systémových otázek. |
| longRetryInterval |Časový interval |00:00:00 |Hello prodleva mezi pokusy o opakování dlouho |

### <a name="typeproperties-section"></a>části v rámci typeProperties
část rámci typeProperties Hello se liší pro každou aktivitu. Transformace aktivity mají pouze vlastnosti typu hello. V tématu [aktivit TRANSFORMACE dat](#data-transformation-activities) v tomto článku pro ukázky JSON, které definují aktivit transformace v datovém kanálu. 

**Aktivita kopírování** má dvě témata v rámci typeProperties části hello: **zdroj** a **podřízený**. V tématu [ÚLOŽIŠŤ dat](#data-stores) pro JSON ukázky, zobrazující jak jako zdroj a jímka úložiště toouse datové části v tomto článku. 

### <a name="sample-copy-pipeline"></a>Ukázkový kanál kopírování
V hello následující ukázkový kanál služby, je jedna aktivita typu **kopie** v hello **aktivity** části. V této ukázce hello [aktivity kopírování](data-factory-data-movement-activities.md) zkopíruje data z Azure Blob storage tooan Azure SQL database. 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Všimněte si hello následující body:

* V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**.
* Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**.
* V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello.

V tématu [ÚLOŽIŠŤ dat](#data-stores) pro JSON ukázky, zobrazující jak jako zdroj a jímka úložiště toouse datové části v tomto článku.    

Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="sample-transformation-pipeline"></a>Ukázkový kanál transformace
V hello následující ukázkový kanál služby, je jedna aktivita typu **HDInsightHive** v hello **aktivity** části. V této ukázce hello [aktivitu HDInsight Hive](data-factory-hive-activity.md) transformuje data z úložiště objektů Blob v Azure tak, že spustíte soubor skriptu Hive v clusteru Azure HDInsight Hadoop. 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

Všimněte si hello následující body: 

* V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**HDInsightHive**.
* soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **AzureStorageLinkedService**) a v  **skript** složky v kontejneru hello **adfgetstarted**.
* Hello **definuje** část se nastavení používané toospecify hello běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

V tématu [aktivit TRANSFORMACE dat](#data-transformation-activities) v tomto článku pro ukázky JSON, které definují aktivit transformace v datovém kanálu.

Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: vytvoření vaší první dat tooprocess kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md). 

## <a name="linked-service"></a>Propojená služba
Hello základní strukturu pro definici propojené služby je následující:

```json
{
    "name": "<name of hello linked service>",
    "properties": {
        "type": "<type of hello linked service>",
        "typeProperties": {
        }
    }
}
```

Následující tabulky popisují hello vlastnosti v rámci aktivity hello definici JSON:

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- | 
| jméno | Název hello propojené služby. | Ano | 
| vlastnosti – typ | Typ hello propojené služby. Příklad: úložiště Azure, Azure SQL Database. |
| typeProperties | Hello rámci typeProperties oddíl obsahuje prvky, které se liší u každé úložiště dat nebo výpočetní prostředí. V tématu [úložišť dat](#datastores) části pro všechna data hello ukládání propojené služby a [výpočetní prostředí](#compute-environments) pro všechny hello výpočetní propojené služby |   

## <a name="dataset"></a>Datová sada 
Datové sady v Azure Data Factory je definován následujícím způsobem:

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
| jméno | Název datové sady hello. V tématu [Azure Data Factory - pravidla po pojmenování](data-factory-naming-rules.md) pravidla pojmenování. |Ano |Není k dispozici |
| type | Typ hello datovou sadu. Zadejte jeden z typů hello podporovaných službou Azure Data Factory (například: AzureBlob, AzureSqlTable). V tématu [ÚLOŽIŠŤ dat](#data-stores) části pro všechny hello datová úložiště a datové sady typy podporované službou Data Factory. | 
| Struktura | Schéma hello datovou sadu. Obsahuje sloupce, jejich typy, atd. | Ne |Není k dispozici |
| typeProperties | Vlastnosti odpovídající toohello vybraný typ. V tématu [ÚLOŽIŠŤ dat](#data-stores) části Podporované typy a jejich vlastnosti. |Ano |Není k dispozici |
| external | Logická hodnota příznak toospecify, zda datové sady je explicitně produkovaný kanálu objekt pro vytváření dat nebo ne. |Ne |False |
| dostupnosti | Definuje hello zpracování okno nebo hello řezů model pro produkční hello datovou sadu. Podrobnosti pro datovou sadu hello řezů modelu najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md) článku. |Ano |Není k dispozici |
| policy |Definuje kritéria hello nebo hello podmínku, která musíte splnit řezy hello datovou sadu. <br/><br/>Podrobnosti najdete v tématu [datovou sadu zásad](#Policy) části. |Ne |Není k dispozici |

Každý sloupec v hello **struktura** část obsahuje hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název sloupce hello. |Ano |
| type |Datový typ sloupce hello.  |Ne |
| Jazyková verze |.NET na základě jazykovou verzi toobe používají, pokud je zadaný typ a je typ formátu .NET `Datetime` nebo `Datetimeoffset`. Výchozí hodnota je `en-us`. |Ne |
| Formát |Formátování řetězce toobe používají, pokud je zadaný typ a je typ formátu .NET `Datetime` nebo `Datetimeoffset`. |Ne |

V následující ukázka hello, hello datová sada má tři sloupce `slicetimestamp`, `projectname`, a `pageviews` a jsou typu: řetězec, řetězec a desetinných v uvedeném pořadí.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Hello následující tabulka popisuje vlastnosti, které můžete použít v hello **dostupnosti** části:

| Vlastnost | Popis | Požaduje se | Výchozí |
| --- | --- | --- | --- |
| frequency |Určuje časovou jednotku hello k produkci řez datovou sadu.<br/><br/><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc |Ano |Není k dispozici |
| interval |Určuje multiplikátor pro četnost<br/><br/>"Frekvence x interval" Určuje, jak často hello se vytvářejí.<br/><br/>Pokud třeba hello datovou sadu toobe rozříznut hodinu, nastavíte <b>frekvence</b> příliš<b>hodinu</b>, a <b>interval</b> příliš<b>1</b>.<br/><br/><b>Poznámka:</b>: Pokud zadáte četnost jako minutu, doporučujeme, abyste nastavili hello interval toono méně než 15 |Ano |Není k dispozici |
| Styl |Určuje, zda by měl být na hello počáteční nebo koncové intervalu hello předložen hello řez.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Pokud je nastavena frekvence tooMonth a je nastaven styl tooEndOfInterval, hello se vytvářejí na hello poslední den v měsíci. Pokud je styl hello nastavená tooStartOfInterval, hello se vytvářejí na hello první den v měsíci.<br/><br/>Pokud je nastavena frekvence tooDay a je nastaven styl tooEndOfInterval, hello se vytvářejí v hello poslední hodiny dne hello.<br/><br/>Pokud je nastavena frekvence tooHour a je nastaven styl tooEndOfInterval, hello se vytvářejí na konci hello hello hodina. Například pro řez dobu 13: 00 – 14: 00, hello se vytvářejí na 14: 00. |Ne |EndOfInterval |
| anchorDateTime |Definuje hello absolutní pozici v čase, které používají scheduler toocompute datovou sadu řez hranic. <br/><br/><b>Poznámka:</b>: Pokud má hello AnchorDateTime částí data, která jsou podrobnější než frekvence hello pak hello podrobnější části jsou ignorovány. <br/><br/>Například, pokud hello <b>interval</b> je <b>každou hodinu</b> (frekvence: hodin a interval: 1) a hello <b>AnchorDateTime</b> obsahuje <b>minuty a sekundy</b>pak hello <b>minuty a sekundy</b> částí hello AnchorDateTime jsou ignorovány. |Ne |01/01/0001 |
| Posun |Časový interval, ve které hello začátku a konci všech řezech datovou sadu posunuty. <br/><br/><b>Poznámka:</b>: Pokud jsou zadané anchorDateTime i posun, výsledkem hello je hello kombinaci shift. |Ne |Není k dispozici |

Hello následující části dostupnosti Určuje, že hello výstupní datové sady je buď vytvořené každou hodinu (nebo) vstupní datovou sadu každou hodinu je k dispozici:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Hello **zásad** oddíl v definici datové sady definuje kritéria hello nebo hello podmínku, která hello řezy datovou sadu musí splnit.

| Název zásady | Popis | Použít příliš| Požaduje se | Výchozí |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Ověří, zda hello data ve **objektů blob v Azure** hello splňuje požadavky na minimální velikost (v megabajtech). |Azure Blob |Ne |Není k dispozici |
| minimumRows |Ověří, zda hello data ve **Azure SQL database** nebo **tabulky Azure** obsahuje hello minimální počet řádků. |<ul><li>Azure SQL Database</li><li>Tabulky Azure</li></ul> |Ne |Není k dispozici |

**Příklad:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

Není-li datovou sadu se vytváří pomocí Azure Data Factory, by měl být označen jako **externí**. Toto nastavení obecně platí toohello vstupy první aktivitu v kanálu, pokud aktivita nebo řetězení kanálu je používán.

| Name (Název) | Popis | Požaduje se | Výchozí hodnota |
| --- | --- | --- | --- |
| dataDelay |Zkontrolujte hello čas toodelay na dostupnost hello hello externích dat pro danou řez hello. Například pokud hello data nejsou k dispozici každou hodinu, hello kontrola toosee hello externích dat je k dispozici a hello odpovídající řez je připravený lze zpozdit pomocí dataDelay.<br/><br/>Toohello platí pouze aktuální čas.  Například pokud je 1:00 PM hned teď a tato hodnota je 10 minut, ověření hello se spustí: 10: 00.<br/><br/>Toto nastavení nemá vliv řezy v posledních hello (řezy s řez koncový čas + dataDelay < teď) jsou zpracovávány bez jakéhokoli zpoždění.<br/><br/>Čas větší než 23:59 toospecified pomocí hello je nutné dobu `day.hours:minutes:seconds` formátu. Například toospecify 24 hodin, nepoužívejte 24:00:00; Místo toho použijte 1.00:00:00. Pokud používáte 24:00:00, bude považován za 24 dní (24.00:00:00). 1 den a 4 hodiny zadejte 1:04:00:00. |Ne |0 |
| RetryInterval |Doba čekání Hello mezi selhání a hello další opakujte pokus. Pokud zkuste to nezdaří, zkuste další hello je po retryInterval. <br/><br/>Pokud je 1:00 PM nyní, můžeme začít hello prvního pokusu. Pokud hello trvání toocomplete hello první ověření kontrola je 1 minuta a hello operace se nezdařila, hello další pokus proběhne v 1:00 + 1 min (doba trvání) + 1 min (interval opakování) = 1:02 PM. <br/><br/>Řezy v posledních hello neexistuje žádné zpoždění není. Hello opakování dojde okamžitě. |Ne |00:01:00 (1 min) |
| retryTimeout |Hello časový limit pro jednotlivé pokusy o opakování.<br/><br/>Pokud je tato vlastnost nastavená too10 minut hello toobe potřeby ověření dokončeny v rámci 10 minut. Pokud trvá déle než 10 minut tooperform hello ověření, opakujte hello časový limit.<br/><br/>Pokud všechny pokusy o ověření hello časového limitu, hello řez je označena jako TimedOut. |Ne |00:10:00 (10 minut) |
| maximumRetry |Počet opakování toocheck hello dostupnost externích dat hello. Hello povolená, maximální hodnota je 10. |Ne |3 |


## <a name="data-stores"></a>DATOVÁ ÚLOŽIŠTĚ
Hello [propojená služba](#linked-service) části zadat popis pro elementy JSON, které jsou uvedeny běžné tooall typy propojené služby. Tato část obsahuje podrobnosti o JSON prvky, které jsou specifické tooeach úložišti.

Hello [datovou sadu](#dataset) části zadat popis pro elementy JSON, které jsou uvedeny běžné typy tooall datových sad. Tato část obsahuje podrobnosti o JSON prvky, které jsou specifické tooeach úložišti.

Hello [aktivity](#activity) části zadat popis pro elementy JSON, které jsou uvedeny běžné typy tooall aktivit. Tato část obsahuje podrobnosti o JSON prvky, které jsou konkrétní tooeach úložiště dat, pokud se používá jako zdroj/jímka v aktivitě kopírování.  

Klikněte na odkaz hello hello úložiště mají zájem o toosee hello JSON schémata pro propojenou službu, datové sady a hello zdroj/jímka pro aktivitu kopírování hello.

| Kategorie | Úložiště dat 
|:--- |:--- |
| **Azure** |[Azure Blob Storage](#azure-blob-storage) |
| &nbsp; |[Azure Data Lake Store](#azure-datalake-store) |
| &nbsp; |[Azure Cosmos DB](#azure-cosmos-db) |
| &nbsp; |[Azure SQL Database](#azure-sql-database) |
| &nbsp; |[Azure SQL Data Warehouse](#azure-sql-data-warehouse) |
| &nbsp; |[Azure Search](#azure-search) |
| &nbsp; |[Azure Table storage](#azure-table-storage) |
| **Databáze** |[Amazon Redshift](#amazon-redshift) |
| &nbsp; |[IBM DB2](#ibm-db2) |
| &nbsp; |[MySQL](#mysql) |
| &nbsp; |[Oracle](#oracle) |
| &nbsp; |[PostgreSQL](#postgresql) |
| &nbsp; |[SAP Business Warehouse](#sap-business-warehouse) |
| &nbsp; |[SAP HANA](#sap-hana) |
| &nbsp; |[SQL Server](#sql-server) |
| &nbsp; |[Sybase](#sybase) |
| &nbsp; |[Teradata](#teradata) |
| **NoSQL** |[Cassandra](#cassandra) |
| &nbsp; |[MongoDB](#mongodb) |
| **File** |[Amazon S3](#amazon-s3) |
| &nbsp; |[Systém souborů](#file-system) |
| &nbsp; |[FTP](#ftp) |
| &nbsp; |[HDFS](#hdfs) |
| &nbsp; |[SFTP](#sftp) |
| **Ostatní** |[HTTP](#http) |
| &nbsp; |[OData](#odata) |
| &nbsp; |[ODBC](#odbc) |
| &nbsp; |[Salesforce](#salesforce) |
| &nbsp; |[Webové tabulky](#web-table) |

## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="linked-service"></a>Propojená služba
Existují dva typy propojené služby: propojená služba Azure Storage a propojená služba Azure Storage SAS.

#### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
toolink tooa účet úložiště Azure datovou továrnu pomocí hello **klíč účtu**, vytvoření služby Azure Storage, propojené. toodefine Azure Storage, propojené služby, sada hello **typ** hello propojené služby příliš**azurestorage**. Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect tooAzure úložiště. |Ano |

##### <a name="example"></a>Příklad  

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

#### <a name="azure-storage-sas-linked-service"></a>Propojená služba Azure Storage SAS
Hello SAS úložiště Azure, propojené služby umožňuje toolink služby Azure data factory tooan účet úložiště Azure pomocí sdíleného přístupového podpisu (SAS). Poskytuje objekt pro vytváření dat hello přístup omezený nebo časově vázaných tooall nebo konkrétní prostředky (kontejner nebo objektů blob) v úložišti hello. toolink tooa účet úložiště Azure datovou továrnu pomocí sdíleného přístupového podpisu vytvoření služby Azure úložiště SAS propojený. toodefine Azure úložiště SAS propojená služba, sada hello **typ** hello propojené služby příliš**AzureStorageSas**. Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:   

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| sasUri |Zadejte identifikátor URI podpis sdíleného přístupu toohello Azure Storage prostředky jako objekt blob, kontejneru nebo tabulky. |Ano |

##### <a name="example"></a>Příklad

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Další informace o těchto propojených služeb najdete v tématu [konektor Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Azure Blob sadu hello **typ** sady dat hello příliš**AzureBlob**. Potom můžete určit následující konkrétní vlastnosti objektů Blob v Azure v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Cesta toohello kontejneru a složce v úložišti objektů blob hello. Příklad: myblobcontainer\myblobfolder\ |Ano |
| fileName |Název objektu hello blob. Název souboru je volitelné a velká a malá písmena.<br/><br/>Pokud určíte název souboru, hello aktivitu (včetně kopie) funguje na hello konkrétní objekt Blob.<br/><br/>Pokud není zadán název souboru, zahrnuje kopírování všech objektů BLOB v hello folderPath pro vstupní datové sady.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| partitionedBy |partitionedBy vlastnost je volitelná. Můžete ho toospecify dynamické folderPath a název souboru pro data časové řady. Například folderPath lze nastavit parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

#### <a name="example"></a>Příklad

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


Další informace najdete v tématu [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties) článku.

### <a name="blobsource-in-copy-activity"></a>BlobSource v aktivitě kopírování
Pokud jsou kopírování dat z Azure Blob Storage, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**BlobSource**a zadejte následující vlastnosti v hello ** zdroj ** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky. |True (výchozí hodnota), False. |Ne |

#### <a name="example-blobsource"></a>Příklad: BlobSource **
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a>BlobSink v aktivitě kopírování
Pokud kopírujete data tooan Azure Blob Storage, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**BlobSink**a zadejte následující vlastnosti v hello **podřízený** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| copyBehavior |Definuje chování kopie hello, pokud je zdroj hello BlobSource nebo systému souborů. |<b>PreserveHierarchy</b>: uchovává hello hierarchií souborů v cílové složce hello. relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.<br/><br/><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello jsou v hello první úroveň cílové složce. Hello zaměřením mít název automaticky generovány. <br/><br/><b>MergeFiles (výchozí):</b> slučuje všechny soubory ze hello zdrojové složky tooone souboru. Pokud je zadán hello název souboru nebo objekt Blob, název sloučené souboru hello by být zadaný název hello; jinak by automaticky generovaný soubor název. |Ne |

#### <a name="example-blobsink"></a>Příklad: BlobSink

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Další informace najdete v tématu [konektor Azure Blob](data-factory-azure-blob-connector.md#copy-activity-properties) článku. 

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Propojená služba
toodefine služby Azure Data Lake Store propojené sady hello typ hello propojená služba příliš**AzureDataLakeStore**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | vlastnost typu Hello musí být nastavena na: **AzureDataLakeStore** | Ano |
| dataLakeStoreUri | Zadejte informace o hello účtu Azure Data Lake Store. Je ve formátu hello: `https://[accountname].azuredatalakestore.net/webhdfs/v1` nebo `adl://[accountname].azuredatalakestore.net/`. | Ano |
| subscriptionId | Předplatné Azure Id toowhich Data Lake Store patří. | Vyžaduje se pro sink |
| Název skupiny prostředků | Patří toowhich název skupiny prostředků Azure Data Lake Store. | Vyžaduje se pro sink |
| servicePrincipalId | Zadejte ID aplikace hello klienta. | Ano (pro objekt zabezpečení ověřování služby) |
| servicePrincipalKey | Zadejte klíč aplikace hello. | Ano (pro objekt zabezpečení ověřování služby) |
| Klienta | Zadejte informace klienta hello (název nebo klienta domény ID) v rámci které se nachází aplikace. Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure. | Ano (pro objekt zabezpečení ověřování služby) |
| Autorizace | Klikněte na tlačítko **Authorize** tlačítka na hello **editoru služby Data Factory** a zadejte svoje přihlašovací údaje, který přiřazuje hello automaticky generovaný autorizace URL toothis vlastnost. | Ano (pro ověření přihlašovacích údajů uživatele)|
| ID relace | Id relace OAuth z autorizační relace, hello OAuth. Každé id relace je jedinečné a může být použit pouze jednou. Toto nastavení se automaticky generuje při pomocí editoru služby Data Factory. | Ano (pro ověření přihlašovacích údajů uživatele) |

#### <a name="example-using-service-principal-authentication"></a>Příklad: použití ověřování hlavní služby
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a>Příklad: použití ověřování pověření uživatele
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Azure Data Lake Store sadu hello **typ** sady dat hello příliš**AzureDataLakeStore**a zadejte následující vlastnosti v hello hello **rámci typeProperties**části: 

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| folderPath |Uložit cestu toohello kontejneru a složce v hello Azure Data Lake. |Ano |
| fileName |Název souboru hello v hello Azure Data Lake store. Název souboru je volitelné a velká a malá písmena. <br/><br/>Pokud zadáte název souboru, na konkrétní soubor hello funguje hello aktivitu (včetně kopie).<br/><br/>Pokud není zadán název souboru, kopie zahrnuje všechny soubory v hello folderPath pro vstupní datové sady.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| partitionedBy |partitionedBy vlastnost je volitelná. Můžete ho toospecify dynamické folderPath a název souboru pro data časové řady. Například folderPath lze nastavit parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

#### <a name="example"></a>Příklad
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
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

Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#dataset-properties) článku. 

### <a name="azure-data-lake-store-source-in-copy-activity"></a>Azure Data Lake Store zdroj v aktivitě kopírování
Pokud jsou kopírování dat z Azure Data Lake Store, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**AzureDataLakeStoreSource**a zadejte následující vlastnosti v hello **zdroje**  části:

**AzureDataLakeStoreSource** podporuje následující vlastnosti hello **rámci typeProperties** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky. |True (výchozí hodnota), False. |Ne |

#### <a name="example-azuredatalakestoresource"></a>Příklad: AzureDataLakeStoreSource

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#copy-activity-properties) článku.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>Podřízený Azure Data Lake Store v aktivitě kopírování
Pokud kopírujete data tooan Azure Data Lake Store, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**AzureDataLakeStoreSink**a zadejte následující vlastnosti v hello **podřízený**části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| copyBehavior |Určuje chování kopie hello. |<b>PreserveHierarchy</b>: uchovává hello hierarchií souborů v cílové složce hello. relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.<br/><br/><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello se vytvoří v hello první úroveň cílové složce. Hello zaměřením jsou vytvořen s názvem automaticky generovány.<br/><br/><b>MergeFiles</b>: sloučí všechny soubory ze hello zdrojové složky tooone souboru. Pokud je zadán hello název souboru nebo objekt Blob, název sloučené souboru hello by být zadaný název hello; jinak by automaticky generovaný soubor název. |Ne |

#### <a name="example-azuredatalakestoresink"></a>Příklad: AzureDataLakeStoreSink
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
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
        }]
    }
}
```

Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#copy-activity-properties) článku. 

## <a name="azure-cosmos-db"></a>Azure Cosmos DB  

### <a name="linked-service"></a>Propojená služba
toodefine Azure DB Cosmos propojená služba, sada hello **typ** hello propojené služby příliš**DocumentDb**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| **Vlastnost** | **Popis** | **Požadované** |
| --- | --- | --- |
| připojovací řetězec |Zadejte informace potřebné tooconnect tooAzure Cosmos DB databáze. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Azure Cosmos DB sady hello **typ** sady dat hello příliš**DocumentDbCollection**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| **Vlastnost** | **Popis** | **Požadované** |
| --- | --- | --- |
| Název_kolekce |Název hello kolekce Azure Cosmos DB. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#dataset-properties) článku.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>Zdroj kolekce Azure Cosmos DB v aktivitě kopírování
Pokud kopírujete data z databáze Cosmos Azure, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**DocumentDbCollectionSource**a zadejte následující vlastnosti v hello **zdroj** části:


| **Vlastnost** | **Popis** | **Povolené hodnoty** | **Požadované** |
| --- | --- | --- | --- |
| query |Zadejte hello dotazu tooread data. |Řetězec nepodporuje Azure Cosmos DB dotazu. <br/><br/>Příklad:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Ne <br/><br/>Pokud není zadaný, hello příkaz jazyka SQL, který se spustí:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Je vnořený tooindicate speciální znak, který hello dokumentu |Libovolný znak. <br/><br/>Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury. Azure Data Factory umožňuje hierarchie toodenote uživatele prostřednictvím nestingSeparator, což je "." v hello výše příklady. S oddělovačem hello aktivity kopírování hello vygeneruje hello "Name" objekt s tří podřízených elementů první, střední a poslední, podle too"Name.First", "Name.Middle" a "Name.Last" v hello Definice tabulky. |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Azure Cosmos DB kolekce podřízený v aktivitě kopírování
Pokud kopírujete data tooAzure Cosmos DB, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**DocumentDbCollectionSink**a zadejte následující vlastnosti v hello **podřízený**části:

| **Vlastnost** | **Popis** | **Povolené hodnoty** | **Požadované** |
| --- | --- | --- | --- |
| nestingSeparator |Je potřeba speciálního znaku v hello zdrojový sloupec název tooindicate, který vnořených dokumentů. <br/><br/>Například výše: `Name.First` ve výstupu hello tabulky vytváří hello strukturu JSON v dokumentu Cosmos DB hello:<br/><br/>"Název": {<br/>    "První": "Jan"<br/>}, |Znak, který je použité tooseparate vnořených úrovní.<br/><br/>Výchozí hodnota je `.` (tečka). |Znak, který je použité tooseparate vnořených úrovní. <br/><br/>Výchozí hodnota je `.` (tečka). |
| writeBatchSize |Počet paralelní požadavků tooAzure Cosmos DB služby toocreate dokumenty.<br/><br/>Při kopírování dat z Azure Cosmos DB pomocí této vlastnosti lze optimalizovat výkon hello. Lepšího výkonu můžete očekávat, když zvýšíte writeBatchSize, protože se odesílají další paralelní požadavky tooAzure Cosmos DB. Ale budete potřebovat tooavoid omezení, který lze vyvolat hello chybová zpráva: "Požadavků je velká".<br/><br/>Omezení je určeno podle počtu faktorů, včetně velikosti dokumentů, počet podmínky v dokumentech, indexování zásad cílovou kolekci, atd. Pro operace kopírování, můžete použít lepší hello toohave kolekce (například S3) většina propustnost, které jsou k dispozici (2 500 žádostí jednotek za sekundu). |Integer |Ne (výchozí: 5) |
| writeBatchTimeout |Doba pro operaci toocomplete hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: "00: 30:00" (30 minut). |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#copy-activity-properties) článku.

## <a name="azure-sql-database"></a>Azure SQL Database

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Azure SQL Database **typ** hello propojené služby příliš**azuresqldatabase**a zadejte následující vlastnosti v hello **rámci typeProperties**části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Database instance. |Ano |

#### <a name="example"></a>Příklad
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

Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Azure SQL Database set hello **typ** sady dat hello příliš**AzureSqlTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název hello tabulku nebo zobrazení hello Azure SQL Database instance, kterou propojená služba odkazuje. |Ano |

#### <a name="example"></a>Příklad

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
Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#dataset-properties) článku. 

### <a name="sql-source-in-copy-activity"></a>Zdroje SQL v aktivitě kopírování
Pokud kopírujete data z databáze SQL Azure, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**SqlSource**a zadejte následující vlastnosti v hello **zdroj** části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| sqlReaderQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Příklad: `select * from MyTable`. |Ne |
| sqlReaderStoredProcedureName |Název hello uložené procedury, která čte data z hello zdrojové tabulky. |Název hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }]
    }
}
```
Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#copy-activity-properties) článku. 

### <a name="sql-sink-in-copy-activity"></a>Jímku SQL v aktivitě kopírování
Pokud kopírujete tooAzure dat SQL Database, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**SqlSink**a zadejte následující vlastnosti v hello **podřízený** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| writeBatchTimeout |Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: "00: 30:00" (30 minut). |Ne |
| writeBatchSize |Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello. |Celé číslo (počet řádků) |Ne (výchozí: 10000) |
| sqlWriterCleanupScript |Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez. |Příkaz dotazu. |Ne |
| sliceIdentifierColumnName |Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit. |Název sloupce sloupce s datovým typem binary(32). |Ne |
| sqlWriterStoredProcedureName |Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello. |Název hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |
| sqlWriterTableType |Zadejte toobe tabulku typu název používá v hello uložené procedury. Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky. Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty. |Zadejte název tabulky. |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
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
        }]
    }
}
```

Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#copy-activity-properties) článku. 

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Azure SQL Data Warehouse **typ** hello propojené služby příliš**AzureSqlDW**a zadejte následující vlastnosti v hello **rámci typeProperties**části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Data Warehouse instance. |Ano |



#### <a name="example"></a>Příklad

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Azure SQL Data Warehouse sadu hello **typ** sady dat hello příliš**AzureSqlDWTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties**části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název hello tabulku nebo zobrazení v databázi Azure SQL Data Warehouse hello, která hello propojená služba odkazuje. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
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

Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) článku. 

### <a name="sql-dw-source-in-copy-activity"></a>Zdroj datového skladu SQL v aktivitě kopírování
Pokud jsou kopírování dat z Azure SQL Data Warehouse, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**SqlDWSource**a zadejte následující vlastnosti v hello **zdroj**části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| sqlReaderQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `select * from MyTable`. |Ne |
| sqlReaderStoredProcedureName |Název hello uložené procedury, která čte data z hello zdrojové tabulky. |Název hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
        }]
    }
}
```

Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) článku. 

### <a name="sql-dw-sink-in-copy-activity"></a>Podřízený datového skladu SQL v aktivitě kopírování
Pokud kopírujete tooAzure dat SQL Data Warehouse, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**SqlDWSink**a zadejte následující vlastnosti v hello **podřízený** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez. |Příkaz dotazu. |Ne |
| allowPolyBase |Určuje, zda toouse PolyBase (v případě potřeby) namísto mechanismus hromadné vložení. <br/><br/> **Hello doporučená způsob tooload dat do SQL Data Warehouse pomocí PolyBase je.** |True <br/>NEPRAVDA (výchozí) |Ne |
| polyBaseSettings |Skupinu vlastností, které je možné zadat při hello **allowPolybase** vlastnost je nastavena příliš**true**. |&nbsp; |Ne |
| rejectValue |Určuje číslo hello nebo podíl řádků, které může být odmítnutá před hello se dotaz nezdaří. <br/><br/>Další informace o hello PolyBase odmítnout možnosti v hello **argumenty** části [vytvořit EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) tématu. |0 (výchozí), 1, 2... |Ne |
| rejectType |Určuje, zda je zadána možnost rejectValue hello literálovou hodnotou nebo jako procento. |Hodnota (výchozí), procento |Ne |
| rejectSampleValue |Určuje hello počet řádků tooretrieve před hello PolyBase přepočítá hello procento odmítnutých řádků. |1, 2, … |Ano, pokud **rejectType** je **procento** |
| useTypeDefault |Určuje, jak toohandle chybějící hodnoty v oddělené textové soubory, když PolyBase načítá data z textového souboru hello.<br/><br/>Další informace o této vlastnosti z oddílu argumenty hello v [vytvořit EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |Hodnota TRUE, False (výchozí) |Ne |
| writeBatchSize |Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello |Celé číslo (počet řádků) |Ne (výchozí: 10000) |
| writeBatchTimeout |Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: "00: 30:00" (30 minut). |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
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
        }]
    }
}
```

Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) článku. 

## <a name="azure-search"></a>Azure Search

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Azure Search **typ** hello propojené služby příliš**AzureSearch**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| Adresa URL | Adresa URL pro hello služby Azure Search. | Ano |
| key | Klíč správce pro hello služby Azure Search. | Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Azure Search sadu hello **typ** sady dat hello příliš**AzureSearchIndex**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části : 

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| type | musí být nastavena vlastnost typu Hello příliš**AzureSearchIndex**.| Ano |
| indexName | Název indexu Azure Search hello. Objekt pro vytváření dat nevytváří hello index. Hello index musí existovat ve službě Azure Search. | Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#dataset-properties) článku.

### <a name="azure-search-index-sink-in-copy-activity"></a>Podřízený Index Azure Search v aktivitě kopírování
Pokud kopírujete indexu Azure Search tooan dat, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**AzureSearchIndexSink**a zadejte následující vlastnosti v hello **podřízený**části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Určuje, zda toomerge nebo nahradit, když dokumentu již existuje v indexu hello. | Merge (výchozí)<br/>Odeslat| Ne |
| writeBatchSize | Ukládání dat do indexu Azure Search hello, když velikost vyrovnávací paměti hello dosáhne writeBatchSize. | 1 too1 000. Výchozí hodnota je 1 000. | Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
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
        }]
    }
}
```

Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#copy-activity-properties) článku.

## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="linked-service"></a>Propojená služba
Existují dva typy propojené služby: propojená služba Azure Storage a propojená služba Azure Storage SAS.

#### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
toolink tooa účet úložiště Azure datovou továrnu pomocí hello **klíč účtu**, vytvoření služby Azure Storage, propojené. toodefine Azure Storage, propojené služby, sada hello **typ** hello propojené služby příliš**azurestorage**. Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |vlastnost typu Hello musí být nastavena na: **azurestorage.** |Ano |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect tooAzure úložiště. |Ano |

**Příklad:**  

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

#### <a name="azure-storage-sas-linked-service"></a>Propojená služba Azure Storage SAS
Hello SAS úložiště Azure, propojené služby umožňuje toolink služby Azure data factory tooan účet úložiště Azure pomocí sdíleného přístupového podpisu (SAS). Poskytuje objekt pro vytváření dat hello přístup omezený nebo časově vázaných tooall nebo konkrétní prostředky (kontejner nebo objektů blob) v úložišti hello. toolink tooa účet úložiště Azure datovou továrnu pomocí sdíleného přístupového podpisu vytvoření služby Azure úložiště SAS propojený. toodefine Azure úložiště SAS propojená služba, sada hello **typ** hello propojené služby příliš**AzureStorageSas**. Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:   

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |vlastnost typu Hello musí být nastavena na: **AzureStorageSas** |Ano |
| sasUri |Zadejte identifikátor URI podpis sdíleného přístupu toohello Azure Storage prostředky jako objekt blob, kontejneru nebo tabulky. |Ano |

**Příklad:**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Azure Table sadu hello **typ** sady dat hello příliš**AzureTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello hello instance Azure tabulku databáze, kterou propojená služba odkazuje. |Ano. Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový. Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový. |

#### <a name="example"></a>Příklad

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
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

Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#dataset-properties) článku. 

### <a name="azure-table-source-in-copy-activity"></a>Zdrojové tabulky Azure v aktivitě kopírování
Pokud jsou kopírování dat z úložiště tabulek Azure, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**AzureTableSource**a zadejte následující vlastnosti v hello **zdroj**části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| azureTableSourceQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu tabulky Azure. Příklady v další části hello. |Ne. Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový. Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový. |
| azureTableSourceIgnoreTableNotFound |Označuje, zda swallow hello výjimky tabulky neexistuje. |HODNOTA TRUE<br/>FALSE |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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
        }]
    }
}
```

Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#copy-activity-properties) článku. 

### <a name="azure-table-sink-in-copy-activity"></a>Podřízený tabulky Azure v aktivitě kopírování
Pokud kopírujete data tooAzure Table Storage, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**AzureTableSink**a zadejte následující vlastnosti v hello **podřízený** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Výchozí hodnotu klíče oddílu, mohou být využívána hello jímky. |Hodnotu řetězce. |Ne |
| azureTablePartitionKeyName |Zadejte název hello sloupce, jejichž hodnoty se používají jako klíče oddílů. Pokud není zadaný, použije se jako klíč oddílu hello AzureTableDefaultPartitionKeyValue. |Název sloupce. |Ne |
| azureTableRowKeyName |Zadejte název hello sloupce, jejichž hodnoty sloupce jsou použity jako klíč řádku. Pokud není zadaný, použijte identifikátor GUID pro každý řádek. |Název sloupce. |Ne |
| azureTableInsertType |Hello režimu tooinsert data do tabulky Azure.<br/><br/>Tato vlastnost určuje, jestli mají existující řádky v tabulce výstup hello s odpovídajícím klíče oddílu a řádku jejich hodnoty nahradit nebo sloučit. <br/><br/>toolearn o tom, jak tato nastavení (sloučení a nahraďte) fungují, najdete v části [vložení nebo sloučení Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) a [vložení nebo nahrazení Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) témata. <br/><br> Toto nastavení se vztahuje na úrovni řádku hello, není hello na úrovni tabulky, a ani možnost odstraní řádky v tabulce výstup hello, které nejsou k dispozici ve vstupu hello. |Merge (výchozí)<br/>Nahradit |Ne |
| writeBatchSize |Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure. |Celé číslo (počet řádků) |Ne (výchozí: 10000) |
| writeBatchTimeout |Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure |Časový interval<br/><br/>Příklad: "00:20:00" (20 minut) |Ne (výchozí hodnota časového limitu pro klienta toostorage výchozí hodnotu 90 sekundu) |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
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
        }]
    }
}
```
Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#copy-activity-properties) článku. 

## <a name="amazon-redshift"></a>Amazon RedShift

### <a name="linked-service"></a>Propojená služba
toodefine Redshift Amazon propojená služba, sada hello **typ** hello propojené služby příliš**AmazonRedshift**a zadejte následující vlastnosti v hello **rámci typeProperties**části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| server |IP adresa nebo název hostitele serveru Amazon Redshift hello. |Ano |
| port |Hello počet hello port TCP, který hello Amazon Redshift server používá toolisten pro připojení klientů. |Ne, výchozí hodnota: 5439 |
| Databáze |Název databáze Amazon Redshift hello. |Ano |
| uživatelské jméno |Jméno uživatele, který má přístup toohello databáze. |Ano |
| heslo |Heslo pro uživatelský účet hello. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Amazon Redshift sadu hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název hello tabulky v databázi hello Amazon Redshift, která propojená služba odkazuje. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |


#### <a name="example"></a>Příklad

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#dataset-properties) článku.

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování 
Pokud jsou kopírování dat z Amazon Redshift, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `select * from MyTable`. |Ne (Pokud **tableName** z **datovou sadu** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#copy-activity-properties) článku.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine IBM DB2 **typ** hello propojené služby příliš**OnPremisesDB2**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| server |Název serveru hello DB2. |Ano |
| Databáze |Název databáze hello DB2. |Ano |
| Schéma |Název schématu hello v databázi hello. název schématu Hello rozlišuje velká a malá písmena. |Ne |
| authenticationType. |Typ ověřování používá databázi DB2 toohello tooconnect. Možné hodnoty jsou: anonymní, základní a systému Windows. |Ano |
| uživatelské jméno |Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi DB2. |Ano |

#### <a name="example"></a>Příklad
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
Datová sada toodefine DB2, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello instance databáze DB2 hello, kterou propojená služba odkazuje. Hello tableName rozlišuje velká a malá písmena. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) 

#### <a name="example"></a>Příklad
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#dataset-properties) článku.

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud jsou kopírování dat z IBM DB2, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `"query": "select * from "MySchema"."MyTable""`. |Ne (Pokud **tableName** z **datovou sadu** je zadána) |

#### <a name="example"></a>Příklad
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#copy-activity-properties) článku.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Propojená služba
toodefine MySQL propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesMySql**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| server |Název serveru MySQL hello. |Ano |
| Databáze |Název databáze MySQL hello. |Ano |
| Schéma |Název schématu hello v databázi hello. |Ne |
| authenticationType. |Typ ověřování používá databázi MySQL toohello tooconnect. Možné hodnoty jsou: `Basic`. |Ano |
| uživatelské jméno |Zadejte uživatelské jméno tooconnect toohello MySQL databázi. |Ano |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali. |Ano |
| gatewayName |Název hello brány, kterou hello služba Data Factory by měl použít toohello tooconnect, místní databázi MySQL. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine MySQL datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v hello instance databáze MySQL, která je propojená služba odkazuje. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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
Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#dataset-properties) článku. 

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud kopírujete data z databáze MySQL, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `select * from MyTable`. |Ne (Pokud **tableName** z **datovou sadu** je zadána) |


#### <a name="example"></a>Příklad
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#copy-activity-properties) článku. 

## <a name="oracle"></a>Oracle 

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Oracle **typ** hello propojené služby příliš**OnPremisesOracle**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| driverType | Určete, jaká data toocopy toouse ovladače z / tooOracle databáze. Povolené hodnoty jsou **Microsoft** nebo **ODP** (výchozí). V tématu [podporované verze a instalace](#supported-versions-and-installation) části na podrobnosti o ovladači. | Ne |
| připojovací řetězec | Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello databáze Oracle instance. | Ano |
| gatewayName | Název hello brány, který je použité tooconnect toohello místního serveru Oracle |Ano |

#### <a name="example"></a>Příklad
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby Oracle sadu hello **typ** sady dat hello příliš**OracleTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v hello databáze Oracle, který hello propojená služba odkazuje. |Ne (Pokud **oracleReaderQuery** z **OracleSource** je zadána) |

#### <a name="example"></a>Příklad

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
            "anchorDateTime": "2016-02-27T12:00:00",
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
Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#dataset-properties) článku.

### <a name="oracle-source-in-copy-activity"></a>Zdroj Oracle v aktivitě kopírování
Pokud kopírujete data z databáze Oracle, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**OracleSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| oracleReaderQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Příklad: `select * from MyTable` <br/><br/>Pokud není zadaný, hello příkaz jazyka SQL, který se spustí:`select * from MyTable` |Ne (Pokud **tableName** z **datovou sadu** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }]
    }
}
```

Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#copy-activity-properties) článku.

### <a name="oracle-sink-in-copy-activity"></a>Podřízený Oracle v aktivitě kopírování
Pokud kopírujete databáze Oracle tooam dat, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**OracleSink**a zadejte následující vlastnosti v hello **podřízený** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| writeBatchTimeout |Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: 00:30:00 (30 minut). |Ne |
| writeBatchSize |Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello. |Celé číslo (počet řádků) |Ne (výchozí: 100) |
| sqlWriterCleanupScript |Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez. |Příkaz dotazu. |Ne |
| sliceIdentifierColumnName |Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit. |Název sloupce sloupce s datovým typem binary(32). |Ne |

#### <a name="example"></a>Příklad
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
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
        }]
    }
}
```
Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#copy-activity-properties) článku.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Propojená služba
toodefine PostgreSQL propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesPostgreSql**a zadejte následující vlastnosti v hello **rámci typeProperties**části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| server |Název serveru PostgreSQL hello. |Ano |
| Databáze |Název databáze PostgreSQL hello. |Ano |
| Schéma |Název schématu hello v databázi hello. název schématu Hello rozlišuje velká a malá písmena. |Ne |
| authenticationType. |Typ ověřování používá databázi PostgreSQL toohello tooconnect. Možné hodnoty jsou: anonymní, základní a systému Windows. |Ano |
| uživatelské jméno |Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi PostgreSQL. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
toodefine PostgreSQL datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v hello instance databáze PostgreSQL, která je propojená služba odkazuje. Hello tableName rozlišuje velká a malá písmena. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |

#### <a name="example"></a>Příklad
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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
Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#dataset-properties) článku.

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud kopírujete data z databáze PostgreSQL, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: "dotaz": "vybrat * z \"MySchema\".\" MyTable\"". |Ne (Pokud **tableName** z **datovou sadu** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#copy-activity-properties) článku.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine SAP Business Warehouse (BW) **typ** hello propojené služby příliš**SapBw**a zadejte následující vlastnosti v hello **rámci typeProperties**části:  

Vlastnost | Popis | Povolené hodnoty | Požaduje se
-------- | ----------- | -------------- | --------
server | Název hello serveru, na které hello SAP BW nachází instance. | Řetězec | Ano
systemNumber | Systém počet hello systému SAP BW. | Desetinné číslo letopočty řetězec. | Ano
clientId | ID klienta hello klienta v hello SAP W systému. | Tři číslice desítkové číslo řetězec. | Ano
uživatelské jméno | Název hello uživatele, který má přístup k serveru SAP toohello | Řetězec | Ano
heslo | Heslo pro uživatele hello. | Řetězec | Ano
gatewayName | Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní SAP BW instancí. | Řetězec | Ano
encryptedCredential | Hello šifrovaný řetězec přihlašovacích údajů. | Řetězec | Ne

#### <a name="example"></a>Příklad

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
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

Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine SAP BW datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**. Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP BW hello typu **RelationalTable**.  

#### <a name="example"></a>Příklad

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
Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#dataset-properties) článku. 

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud jsou kopírování dat z SAP Business Warehouse, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query | Určuje hello MDX dotazu tooread data z instance SAP BW hello. | Dotaz MDX. | Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
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
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) článku. 

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine SAP HANA **typ** hello propojené služby příliš**SapHana**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

Vlastnost | Popis | Povolené hodnoty | Požaduje se
-------- | ----------- | -------------- | --------
server | Název hello serveru, na které hello SAP HANA nachází instance. Pokud váš server používá vlastní port, zadejte `server:port`. | Řetězec | Ano
authenticationType. | Typ ověřování. | Řetězec. "Základní" nebo "Systém Windows" | Ano 
uživatelské jméno | Název hello uživatele, který má přístup k serveru SAP toohello | Řetězec | Ano
heslo | Heslo pro uživatele hello. | Řetězec | Ano
gatewayName | Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní SAP HANA instance. | Řetězec | Ano
encryptedCredential | Hello šifrovaný řetězec přihlašovacích údajů. | Řetězec | Ne

#### <a name="example"></a>Příklad

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#linked-service-properties) článku.
 
### <a name="dataset"></a>Datová sada
toodefine SAP HANA datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**. Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP HANA hello typu **RelationalTable**. 

#### <a name="example"></a>Příklad

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#dataset-properties) článku. 

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud jsou kopírování dat z jiného úložiště dat SAP HANA, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query | Určuje hello SQL dotaz tooread data z instance SAP HANA hello. | Dotaz SQL. | Ano |


#### <a name="example"></a>Příklad


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#copy-activity-properties) článku.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Propojená služba
Vytvoření propojené služby typu **onpremisessqlserver** toolink služby místní systém SQL Server databáze tooa data factory. Hello následující tabulka obsahuje popis služby SQL serveru propojená konkrétní tooon místní elementy JSON.

Hello následující tabulka obsahuje popis pro konkrétní tooSQL elementy JSON serveru propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello by měla být nastavena na: **onpremisessqlserver**. |Ano |
| připojovací řetězec |Zadejte požadované informace connectionString tooconnect toohello místní databáze SQL serveru pomocí ověřování SQL nebo ověřování systému Windows. |Ano |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi systému SQL Server. |Ano |
| uživatelské jméno |Zadejte uživatelské jméno, pokud používáte ověřování systému Windows. Příklad: **domainname\\uživatelské jméno**. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |

Můžete šifrovat přihlašovací údaje pomocí hello **New-AzureRmDataFactoryEncryptValue** rutiny a použijte je v hello připojovací řetězec, jak ukazuje následující příklad hello (**EncryptedCredential** vlastnost):  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Příklad: JSON pro pomocí ověřování SQL.

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Příklad: JSON pro použití ověřování systému Windows

Pokud jsou zadané uživatelské jméno a heslo, brána používá, je tooimpersonate hello zadaný uživatelský účet tooconnect toohello místní databázi systému SQL Server. Jinak brána připojí toohello systému SQL Server přímo kontext zabezpečení hello brány (jeho účet spuštění).

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sady SQL Server, sada hello **typ** sady dat hello příliš**SqlServerTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název hello tabulku nebo zobrazení v hello instance databáze SQL serveru, který propojená služba odkazuje. |Ano |

#### <a name="example"></a>Příklad
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
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

Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#dataset-properties) článku. 

### <a name="sql-source-in-copy-activity"></a>Zdroje SQL v aktivitě kopírování
Pokud kopírujete data z databáze systému SQL Server, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**SqlSource**a zadejte následující vlastnosti v hello **zdroj** části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| sqlReaderQuery |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `select * from MyTable`. Může odkazovat více tabulek z databáze hello odkazuje hello vstupní datové sady. Pokud není zadaný, hello příkaz jazyka SQL, která se provedla: Vyberte možnost z MyTable. |Ne |
| sqlReaderStoredProcedureName |Název hello uložené procedury, která čte data z hello zdrojové tabulky. |Název hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |

Pokud hello **sqlReaderQuery** je zadán pro hello SqlSource, hello aktivity kopírování spouští tento dotaz hello databáze systému SQL Server zdrojová tooget hello data.

Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server. Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.

> [!NOTE]
> Při použití **sqlReaderStoredProcedureName**, stále potřebujete toospecify hodnotu pro hello **tableName** vlastnost v datové sadě hello JSON. Neexistují žádné ověření, ale adresovat této tabulky.


#### <a name="example"></a>Příklad
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }]
    }
}
```

V tomto příkladu **sqlReaderQuery** pro hello SqlSource je zadána. Hello aktivity kopírování spouští tento dotaz hello data hello tooget zdrojové databáze systému SQL Server. Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry). Hello sqlReaderQuery může odkazovat více tabulek v rámci hello databáze odkazuje hello vstupní datové sady. Není omezený tooonly hello tabulky nastavena jako hello typeProperty tableName datové sady.

Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server. Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.

Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#copy-activity-properties) článku. 

### <a name="sql-sink-in-copy-activity"></a>Jímku SQL v aktivitě kopírování
Pokud kopírujete databáze systému SQL Server tooa dat, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**SqlSink**a zadejte následující vlastnosti v hello **podřízený** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| writeBatchTimeout |Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit. |Časový interval<br/><br/> Příklad: "00: 30:00" (30 minut). |Ne |
| writeBatchSize |Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello. |Celé číslo (počet řádků) |Ne (výchozí: 10000) |
| sqlWriterCleanupScript |Zadejte dotaz pro aktivitu kopírování tooexecute tak, aby se vyčistit data určitý řez. Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy) části. |Příkaz dotazu. |Ne |
| sliceIdentifierColumnName |Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit. Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy) části. |Název sloupce sloupce s datovým typem binary(32). |Ne |
| sqlWriterStoredProcedureName |Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello. |Název hello uložené procedury. |Ne |
| storedProcedureParameters |Parametry pro hello uložené procedury. |Páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury. |Ne |
| sqlWriterTableType |Zadejte toobe název typu tabulky používán hello uložené procedury. Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky. Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty. |Zadejte název tabulky. |Ne |

#### <a name="example"></a>Příklad
Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu. V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
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
        }]
    }
}
```

Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#copy-activity-properties) článku. 

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Propojená služba
toodefine Sybase propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesSybase**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| server |Název serveru Sybase hello. |Ano |
| Databáze |Název databáze Sybase hello. |Ano |
| Schéma |Název schématu hello v databázi hello. |Ne |
| authenticationType. |Typ ověřování používá databázi Sybase toohello tooconnect. Možné hodnoty jsou: anonymní, základní a systému Windows. |Ano |
| uživatelské jméno |Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi Sybase. |Ano |

#### <a name="example"></a>Příklad
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine Sybase datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v hello instance databáze Sybase, která je propojená služba odkazuje. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#dataset-properties) článku. 

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud kopírujete data z databáze Sybase, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `select * from MyTable`. |Ne (Pokud **tableName** z **datovou sadu** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#copy-activity-properties) článku.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Teradata **typ** hello propojené služby příliš**OnPremisesTeradata**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| server |Název serveru Teradata hello. |Ano |
| authenticationType. |Typ ověřování používá tooconnect toohello Teradata databáze. Možné hodnoty jsou: anonymní, základní a systému Windows. |Ano |
| uživatelské jméno |Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní Teradata databázi. |Ano |

#### <a name="example"></a>Příklad
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
toodefine datovou sadu objektu Teradata Blob, sada hello **typ** sady dat hello příliš**RelationalTable**. Momentálně nejsou k dispozici žádné vlastnosti typu podporované pro datovou sadu hello Teradata. 

#### <a name="example"></a>Příklad
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#dataset-properties) článku.

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud kopírujete data z databáze Teradata, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `select * from MyTable`. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#copy-activity-properties) článku.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Propojená služba
toodefine Cassandra propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesCassandra**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| hostitele |Jeden nebo více IP adres nebo názvů hostitelů Cassandra serverů.<br/><br/>Zadejte seznam IP adres nebo serverům tooall tooconnect názvy hostitele současně. |Ano |
| port |port TCP, který hello Cassandra server Hello používá toolisten pro připojení klientů. |Ne, výchozí hodnota: 9042 |
| authenticationType. |Basic nebo Anonymous |Ano |
| uživatelské jméno |Zadejte uživatelské jméno pro hello uživatelský účet. |Ano, pokud je nastavená authenticationType tooBasic. |
| heslo |Zadejte heslo pro uživatelský účet hello. |Ano, pokud je nastavená authenticationType tooBasic. |
| gatewayName |Název Hello hello bránu, která je použité tooconnect toohello místní Cassandra databázi. |Ano |
| encryptedCredential |Přihlašovací údaje šifrované bránou hello. |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine Cassandra datovou sadu, sada hello **typ** sady dat hello příliš**CassandraTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| keyspace |Název schématu v databázi Cassandra nebo hello keyspace. |Ano (Pokud **dotazu** pro **CassandraSource** není definován). |
| tableName |Název hello tabulky v databázi Cassandra. |Ano (Pokud **dotazu** pro **CassandraSource** není definován). |

#### <a name="example"></a>Příklad

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#dataset-properties) článku. 

### <a name="cassandra-source-in-copy-activity"></a>Zdroj Cassandra v aktivitě kopírování
Pokud jsou kopírování dat z Cassandra, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**CassandraSource**a zadejte následující vlastnosti v hello **zdroj** části :

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Dotaz SQL 92 nebo CQL dotazu. V tématu [CQL odkaz](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Při použití příkazu jazyka SQL, zadejte **keyspace name.table název** toorepresent hello tabulky, které mají být tooquery. |Ne (pokud jsou definovány tableName a keyspace v sadě dat). |
| consistencyLevel |úroveň konzistence Hello Určuje, kolik repliky musí odpovídat požadavků na čtení tooa před vrácením dat toohello klientskou aplikaci. Kontroly Cassandra hello zadaný počet replik pro data toosatisfy hello čtení požadavku. |JEDEN, DVA, TŘI, KVORA, VŠE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. V tématu [konfigurace konzistenci dat](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) podrobnosti. |Ne. Výchozí hodnota je 1. |

#### <a name="example"></a>Příklad
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
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
        }]
    }
}
```

Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#copy-activity-properties) článku.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Propojená služba
toodefine MongoDB propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesMongoDB**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| server |IP adresa nebo název hostitele serveru MongoDB hello. |Ano |
| port |Port TCP, který hello serveru MongoDB toolisten používá pro připojení klientů. |Volitelné, výchozí hodnota: 27017 |
| authenticationType. |Základní, nebo anonymní. |Ano |
| uživatelské jméno |Uživatel účet tooaccess MongoDB. |Ano (Pokud se používá základní ověřování). |
| heslo |Heslo pro uživatele hello. |Ano (Pokud se používá základní ověřování). |
| authSource |Název databáze hello MongoDB, že chcete toouse toocheck přihlašovacích údajů pro ověřování. |Volitelný parametr (Pokud se používá základní ověřování). Výchozí: používá účet správce hello a hello databáze zadat pomocí vlastnost databaseName. |
| Název databáze |Název databáze hello MongoDB, které chcete tooaccess. |Ano |
| gatewayName |Název brány hello, který přistupuje k úložišti dat hello. |Ano |
| encryptedCredential |Přihlašovací údaje zašifrovaná pomocí brány. |Nepovinné |

#### <a name="example"></a>Příklad

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Datová sada
toodefine MongoDB datovou sadu, sada hello **typ** sady dat hello příliš**MongoDbCollection**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Název_kolekce |Název kolekce hello v databázi MongoDB. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "MongoDbInputDataset",
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

Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>Zdroj MongoDB v aktivitě kopírování
Pokud jsou kopírování dat z MongoDB, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**MongoDbSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL 92. Například: `select * from MyTable`. |Ne (Pokud **Název_kolekce** z **datovou sadu** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Amazon S3 **typ** hello propojené služby příliš**AwsAccessKey**a zadejte následující vlastnosti v hello **rámci typeProperties** části :  

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| accessKeyID |ID hello tajný přístupový klíč. |Řetězec |Ano |
| secretAccessKey |Hello tajný přístupový klíč, sám sebe. |Šifrované tajné řetězec |Ano |

#### <a name="example"></a>Příklad
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).

### <a name="dataset"></a>Datová sada
Datová sada toodefine Amazon S3, sada hello **typ** sady dat hello příliš**AmazonS3**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| bucketName |Název sady Hello S3. |Řetězec |Ano |
| key |klíč objektu Hello S3. |Řetězec |Ne |
| Předpona |Předpona pro klíč objektu S3 hello. Jsou vybrané objekty, jejichž klíče začít s touto předponou. Platí pouze v případě, klíč je prázdný. |Řetězec |Ne |
| Verze |Hello verze objektu S3, pokud je povolena Správa verzí S3. |Řetězec |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne | |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. jsou Hello podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne | |


> [!NOTE]
> bucketName + klíče určuje umístění hello hello S3 objektu, kde blok je hello Kořenový kontejner pro objekty S3 a klíč je objekt tooS3 hello úplnou cestu.

#### <a name="example-sample-dataset-with-prefix"></a>Příklad: Ukázkovou datovou sadu s předponou

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a>Příklad: Ukázka datové sady (verze)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a>Příklad: Dynamické cesty pro S3
V ukázce hello používáme pevné hodnoty pro klíče a bucketName vlastnosti v datové sadě hello Amazon S3.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

Můžete mít vypočítat hello klíč a bucketName dynamicky za běhu pomocí systémové proměnné, jako je například SliceStart služby Data Factory.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Můžete provést stejný hello hello předpona vlastnosti datové sadě služby Amazon S3. V tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md) seznam podporované funkce a proměnné.

Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Zdroj systému souborů v aktivitě kopírování
Pokud kopírujete data z Amazonu S3, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části :


| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda seznam toorecursively S3 objekty v adresáři hello. |hodnotu true nebo false |Ne |


#### <a name="example"></a>Příklad


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).

## <a name="file-system"></a>Systém souborů


### <a name="linked-service"></a>Propojená služba
Můžete se propojit místní soubor systému tooan pro vytváření dat Azure s hello **místní souborový Server** propojené služby. Hello následující tabulka obsahuje popis JSON prvky, které jsou specifické toohello místní souborový Server propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |Ujistěte se, že hello vlastnost Typ nastavena příliš**OnPremisesFileServer**. |Ano |
| hostitele |Určuje hello kořenovou cestu hello složky, které chcete toocopy. Použít hello řídicí znak ' \ ' pro speciální znaky v řetězci hello. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady. |Ano |
| ID uživatele |Zadejte ID hello hello uživatele, který má přístup toohello serveru. |Ne (když zvolíte encryptedCredential) |
| heslo |Zadejte hello heslo pro uživatele hello (ID uživatele). |Ne (když zvolíte encryptedCredential |
| encryptedCredential |Zadejte hello šifrovat přihlašovací údaje, které můžete získat spuštěním rutiny New-AzureRmDataFactoryEncryptValue hello. |Ne (když zvolíte toospecify ID uživatele a heslo ve formátu prostého textu) |
| gatewayName |Určuje název hello hello brány, že objekt pro vytváření dat mají používat tooconnect toohello místní souborový server. |Ano |

#### <a name="sample-folder-path-definitions"></a>Ukázka složky cesta definice 
| Scénář | Hostování v definici propojené služby | folderPath v definici datové sady |
| --- | --- | --- |
| Místní složky v počítači brány pro správu dat: <br/><br/>Příklady: D:\\ \* nebo D:\folder\subfolder\\* |D:\\ \\ (pro Data Management Gateway 2.0 nebo novější) <br/><br/> localhost (pro starší verze než Data Management Gateway 2.0) |. \\ \\ nebo složky\\\\podsložky (pro Data Management Gateway 2.0 nebo novější) <br/><br/>D:\\ \\ nebo D:\\\\složky\\\\podsložky (pro brány verzi nižší než 2.0) |
| Vzdálené sdílené složce: <br/><br/>Příklady: \\ \\myserver\\sdílet\\ \* nebo \\ \\myserver\\sdílet\\složky\\podsložky\\* |\\\\\\\\myserver\\\\sdílet |. \\ \\ nebo složky\\\\podsložky |


#### <a name="example-using-username-and-password-in-plain-text"></a>Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a>Příklad: Pomocí encryptedcredential

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#linked-service-properties).

### <a name="dataset"></a>Datová sada
toodefine datovou sadu systému souborů, sada hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Určuje složku toohello cestou hello. Použít hello řídicí znak ' \' pro speciální znaky v řetězci hello. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.<br/><br/>Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času. |Ano |
| fileName |Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor se hello následující formát: <br/><br/>`Data.<Guid>.txt`(Příklad: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Ne |
| fileFilter |Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů. <br/><br/>Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).<br/><br/>Příklad 1: "fileFilter": "* .log"<br/>Příklad 2: "fileFilter": 2016 - 1-?. TXT"<br/><br/>Všimněte si, že fileFilter je použít pro datové sadě služby vstupní sdílení souborů. |Ne |
| partitionedBy |Můžete vytvořit partitionedBy toospecify dynamické folderPath nebo název souboru pro data časové řady. Příkladem je folderPath parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**; a jsou podporované úrovně: **Optimal** a **nejrychlejší**. v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

> [!NOTE]
> Název souboru a fileFilter nelze současně použít.

#### <a name="example"></a>Příklad

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
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

Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Zdroj systému souborů v aktivitě kopírování
Pokud jsou kopírování dat systému souborů, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z podsložky hello nebo pouze z hello zadané složky. |Hodnota TRUE, False (výchozí) |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#copy-activity-properties).

### <a name="file-system-sink-in-copy-activity"></a>Systém souborů jímky v aktivitě kopírování
Pokud kopírujete data tooFile systému, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**FileSystemSink**a zadejte následující vlastnosti v hello **podřízený** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| copyBehavior |Definuje chování kopie hello, pokud je zdroj hello BlobSource nebo systému souborů. |**PreserveHierarchy:** zachovává hello hierarchií souborů v cílové složce hello. Relativní cesta hello hello zdrojového souboru toohello zdrojové složky tedy je hello stejná jako relativní cesta hello hello cílový soubor toohello cílové složky.<br/><br/>**FlattenHierarchy:** všechny soubory ze zdrojové složky hello se vytvoří v hello první úroveň cílové složce. Hello zaměřením jsou vytvořen s názvem generován automaticky.<br/><br/>**MergeFiles:** slučuje všechny soubory ze hello zdrojové složky tooone souboru. Pokud je zadán název název nebo objekt blob souboru hello, hello sloučené soubor je zadaný název hello. Jinak je název automaticky generovaný soubor. |Ne |
Auto-

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#copy-activity-properties).

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine k serveru FTP **typ** hello propojené služby příliš**Server_ftp**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se | Výchozí |
| --- | --- | --- | --- |
| hostitele |Název nebo IP adresa hello serveru FTP |Ano |&nbsp; |
| authenticationType. |Zadejte typ ověřování |Ano |Anonymní, základní |
| uživatelské jméno |Uživatel, který má přístup k serveru toohello FTP |Ne |&nbsp; |
| heslo |Heslo pro uživatele hello (username) |Ne |&nbsp; |
| encryptedCredential |Server FTP hello tooaccess šifrovaném přihlašovacím údaji |Ne |&nbsp; |
| gatewayName |Název hello Brána pro správu dat brány tooconnect tooan místní FTP server |Ne |&nbsp; |
| port |Port, na které hello FTP server naslouchá |Ne |21 |
| enableSsl |Zadejte, zda toouse FTP přes kanál SSL/TLS. |Ne |Hodnota TRUE |
| enableServerCertificateValidation |Zadejte, zda tooenable serveru SSL certifikát ověření, pokud pomocí FTP přes kanál SSL/TLS. |Ne |Hodnota TRUE |

#### <a name="example-using-anonymous-authentication"></a>Příklad: Pomocí anonymní ověřování

```json
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

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a>Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu pro základní ověřování

```json
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

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a>Příklad: Pomocí portu, enableSsl, enableServerCertificateValidation

```json
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

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Příklad: Pomocí encryptedCredential pro ověřování a brány

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
Datová sada toodefine k serveru FTP, sada hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Dílčí cesta toohello složka. Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.<br/><br/>Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času. |Ano 
| fileName |Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: <br/><br/>Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| fileFilter |Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.<br/><br/>Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).<br/><br/>Příklady 1:`"fileFilter": "*.log"`<br/>Příklad 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter se vztahuje vstupní datové sady sdílení souborů. Tato vlastnost není podporována s HDFS. |Ne |
| partitionedBy |partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady. Například folderPath parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**; a jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |
| useBinaryTransfer |Určit, jestli použít režim binární přenosu. Platí pro binárního režimu a false ASCII. Výchozí hodnota: True. Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp. |Ne |

> [!NOTE]
> Název souboru a fileFilter nelze použít současně.

#### <a name="example"></a>Příklad

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
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

Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#dataset-properties) článku.

### <a name="file-system-source-in-copy-activity"></a>Zdroj systému souborů v aktivitě kopírování
Pokud kopírujete data ze serveru FTP, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky. |Hodnota TRUE, False (výchozí) |Ne |

#### <a name="example"></a>Příklad

```json
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
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#copy-activity-properties) článku.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Propojená služba
toodefine HDFS propojená služba, sada hello **typ** hello propojené služby příliš**Hdfs**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavena na: **Hdfs** |Ano |
| URL |Adresa URL toohello HDFS |Ano |
| authenticationType. |Anonymní, nebo Windows. <br><br> toouse **ověřování protokolem Kerberos** HDFS konektor, najdete v části příliš[v této části](#use-kerberos-authentication-for-hdfs-connector) tooset prostředí místní odpovídajícím způsobem. |Ano |
| Uživatelské jméno |Ověřování uživatelského jména pro systém Windows. |Ano (pro ověřování systému Windows) |
| heslo |Heslo pro ověřování systému Windows. |Ano (pro ověřování systému Windows) |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello HDFS. |Ano |
| encryptedCredential |[Nové AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) výstup hello přístup pověření. |Ne |

#### <a name="example-using-anonymous-authentication"></a>Příklad: Pomocí anonymní ověřování

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a>Příklad: Pomocí ověřování systému Windows

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine HDFS datovou sadu, sada hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Složka toohello cesta. Příklad:`myfolder`<br/><br/>Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello. Příklad: pro folder\subfolder, určete složku\\\\podsložky a pro d:\samplefolder, zadejte d:\\\\ukázková_složka.<br/><br/>Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času. |Ano |
| fileName |Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: <br/><br/>Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| partitionedBy |partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady. Příklad: folderPath parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

> [!NOTE]
> Název souboru a fileFilter nelze použít současně.

#### <a name="example"></a>Příklad

```json
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
            "interval": 1
        }
    }
}
```

Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#dataset-properties) článku. 

### <a name="file-system-source-in-copy-activity"></a>Zdroj systému souborů v aktivitě kopírování
Pokud kopírujete data z HDFS, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:

**FileSystemSource** podporuje hello následující vlastnosti:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky. |Hodnota TRUE, False (výchozí) |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
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
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#copy-activity-properties) článku.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine protokolu SFTP **typ** hello propojené služby příliš**Sftp**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- | --- |
| hostitele | Název nebo IP adresa serveru pomocí protokolu SFTP hello. |Ano |
| port |Port, na které hello SFTP server naslouchá. Hello výchozí hodnota je: 21 |Ne |
| authenticationType. |Zadejte typ ověřování. Povolené hodnoty: **základní**, **parametru SshPublicKey**. <br><br> Odkazovat příliš[základní ověřování pomocí](#using-basic-authentication) a [pomocí SSH ověření veřejného klíče](#using-ssh-public-key-authentication) částech na další vlastnosti a ukázky JSON v uvedeném pořadí. |Ano |
| skipHostKeyValidation | Zadejte, zda tooskip hostitele klíče ověřování. | Ne. Hello výchozí hodnota: false |
| hostKeyFingerprint | Zadejte hello prstu hello hostitele klíče. | Ano, pokud hello `skipHostKeyValidation` nastavena toofalse.  |
| gatewayName |Název hello Brána pro správu dat tooconnect tooan místní server pomocí protokolu SFTP. | Ano, pokud kopírování dat z místního serveru pomocí protokolu SFTP. |
| encryptedCredential | Šifrovaný přihlašovací údaj tooaccess hello SFTP server. Automaticky generovaný když zadáte základní ověřování (uživatelské jméno a heslo) nebo ověřování parametru SshPublicKey (uživatelské jméno + cesta privátního klíče nebo obsahu) v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog. | Ne. Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP. |

#### <a name="example-using-basic-authentication"></a>Příklad: Použití základního ověřování

toouse základní ověřování, nastavit `authenticationType` jako `Basic`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- | --- |
| uživatelské jméno | Uživatel, který má přístup toohello SFTP server. |Ano |
| heslo | Heslo pro uživatele hello (uživatelské jméno). | Ano |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Příklad: Základní ověřování s šifrované pověření **

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a>Pomocí ověření veřejného klíče SSH: **

toouse základní ověřování, nastavit `authenticationType` jako `SshPublicKey`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- | --- |
| uživatelské jméno |Uživatel, který má přístup toohello SFTP serveru |Ano |
| privateKeyPath | Zadejte absolutní cestu toohello soubor privátního klíče můžete přístup k této brány. | Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`. <br><br> Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP. |
| privateKeyContent | Serializovaná řetězec hello privátní klíče obsahu. Průvodce kopírováním Hello můžete číst soubor privátního klíče hello a extrahování hello privátní klíče obsahu automaticky. Pokud používáte jakékoli jiné nástroje nebo SDK, použijte vlastnost privateKeyPath hello. | Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`. |
| přístupové heslo | Pokud soubor klíče hello je chráněn heslo, zadejte hello průchodu fráze nebo hesla toodecrypt hello privátní klíč. | Ano, pokud je chráněný soubor privátního klíče hello heslo. |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče obsahu **

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

Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby SFTP sadu hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| folderPath |Dílčí cesta toohello složka. Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.<br/><br/>Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času. |Ano |
| fileName |Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: <br/><br/>Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Ne |
| fileFilter |Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.<br/><br/>Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).<br/><br/>Příklady 1:`"fileFilter": "*.log"`<br/>Příklad 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter se vztahuje vstupní datové sady sdílení souborů. Tato vlastnost není podporována s HDFS. |Ne |
| partitionedBy |partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady. Například folderPath parametry pro každou hodinu data. |Ne |
| Formát | jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. <br><br> Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |
| useBinaryTransfer |Určit, jestli použít režim binární přenosu. Platí pro binárního režimu a false ASCII. Výchozí hodnota: True. Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp. |Ne |

> [!NOTE]
> Název souboru a fileFilter nelze použít současně.

#### <a name="example"></a>Příklad

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
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

Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#dataset-properties) článku. 

### <a name="file-system-source-in-copy-activity"></a>Zdroj systému souborů v aktivitě kopírování
Pokud kopírujete z protokolu SFTP zdroje dat, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| Rekurzivní |Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky. |Hodnota TRUE, False (výchozí) |Ne |



#### <a name="example"></a>Příklad

```json
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
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#copy-activity-properties) článku.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine HTTP **typ** hello propojené služby příliš**Http**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Adresa URL | Základní adresa URL toohello webového serveru | Ano |
| authenticationType. | Určuje typ ověřování hello. Povolené hodnoty jsou: **anonymní**, **základní**, **Digest**, **Windows**, **ClientCertificate**. <br><br> V uvedeném pořadí odkazovat toosections dál v této tabulce na další vlastnosti a ukázky JSON pro tyto typy ověřování. | Ano |
| enableServerCertificateValidation | Zadejte, zda, tooenable serveru SSL ověření certifikátu, pokud je zdroj HTTPS webového serveru | Ne, výchozí hodnota je true |
| gatewayName | Název hello Brána pro správu dat tooconnect tooan místní zdroje pomocí protokolu HTTP. | Ano, pokud kopírování dat z místního zdroje HTTP. |
| encryptedCredential | Šifrovaný přihlašovací údaj tooaccess hello koncový bod HTTP. Automaticky vygenerované při konfiguraci hello ověřovací informace v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog. | Ne. Platí jenom v případě, že kopírování dat z místního serveru HTTP. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Příklad: Použití ověřování Basic, ověřování algoritmem Digest nebo systému Windows
Nastavit `authenticationType` jako `Basic`, `Digest`, nebo `Windows`a zadejte následující vlastnosti kromě hello HTTP konektor obecné těch, které jsou zavedené výše hello:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| uživatelské jméno | Uživatelské jméno tooaccess hello koncový bod HTTP. | Ano |
| heslo | Heslo pro uživatele hello (uživatelské jméno). | Ano |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a>Příklad: Použití ověřování ClientCertificate

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

**Příklad: pomocí klientského certifikátu:** Tato propojená služba propojuje vaše data factory tooan místní HTTP webový server. Používá klientský certifikát, který je nainstalován na počítači hello s brána správy dat nainstalována.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
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

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
Datová sada toodefine HTTP, sada hello **typ** sady dat hello příliš**Http**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| relativeUrl | Relativní adresa URL toohello prostředek obsahující hello data. Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby. <br><br> tooconstruct dynamické adresy URL, můžete použít [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md), například: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Ne |
| requestMethod | Metoda HTTP. Povolené hodnoty jsou **získat** nebo **POST**. | Ne. Výchozí hodnota je `GET`. |
| additionalHeaders | Další hlavičky žádosti HTTP. | Ne |
| RequestBody | Text pro požadavek HTTP. | Ne |
| Formát | Pokud chcete, aby toosimply **načtení dat hello z koncový bod HTTP jako-je** bez analýza ho, přeskočte tento formát nastavení. <br><br> Pokud chcete tooparse hello HTTP odpovědi obsahu během kopírování, jsou podporovány následující typy formátu hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly. |Ne |
| Komprese | Zadejte typ hello a úroveň komprese dat hello. Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**. Jsou podporované úrovně: **Optimal** a **nejrychlejší**. Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Ne |

#### <a name="example-using-hello-get-default-method"></a>Příklad: pomocí hello metodu GET (výchozí)

```json
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
            "interval": 1
        }
    }
}
```

#### <a name="example-using-hello-post-method"></a>Příklad: pomocí metody POST hello

```json
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
            "interval": 1
        }
    }
}
```
Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#dataset-properties) článku.

### <a name="http-source-in-copy-activity"></a>Zdroj protokolu HTTP v aktivitě kopírování
Pokud kopírujete data ze zdroje HTTP, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**HttpSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| httpRequestTimeout | Hello vypršení časového limitu (časový interval) pro tooget požadavku HTTP hello odpověď. Je hello časový limit tooget odpověď, nebyla hello časový limit tooread data odpovědi. | Ne. Výchozí hodnota: 00:01:40 |


#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }]
    }
}
```

Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#copy-activity-properties) článku.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine OData **typ** hello propojené služby příliš**OData**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Adresa URL |Adresa URL služby OData hello. |Ano |
| authenticationType. |Typ ověřování používá tooconnect toohello OData zdroje. <br/><br/> Možné hodnoty pro cloudové prostředí OData, jsou anonymní, základní a OAuth (Upozorňujeme, že Azure Active Directory na základě OAuth aktuálně jedinou podpory Azure Data Factory). <br/><br/> Pro místní OData možné hodnoty jsou anonymní, Basic a Windows. |Ano |
| uživatelské jméno |Pokud používáte základní ověřování, zadejte uživatelské jméno. |Ano (jenom Pokud používáte základní ověřování) |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ano (jenom Pokud používáte základní ověřování) |
| authorizedCredential |Pokud používáte OAuth, klikněte na tlačítko **Autorizovat** tlačítko v hello Průvodce kopírováním služby Data Factory editoru nebo editoru a zadejte svoje přihlašovací údaje, pak bude automaticky generovaný hello hodnota této vlastnosti. |Ano (jenom Pokud používáte ověřování OAuth) |
| gatewayName |Název hello brány, kterou hello služba Data Factory měli používat službu tooconnect toohello místní OData. Zadejte, pokud jsou kopírování dat z místní zdroj OData. |Ne |

#### <a name="example---using-basic-authentication"></a>Příklad: použití základního ověřování
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a>Příklad: pomocí anonymní ověřování

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Příklad: přístup k ověřování pomocí Windows místní zdroj OData

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Příklad - použití ověřování OAuth přístup ke cloudu zdroj OData
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#linked-service-properties) článku.

### <a name="dataset"></a>Datová sada
toodefine datové sadě služby OData sadu hello **typ** sady dat hello příliš**ODataResource**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Cesta |Toohello cestu OData prostředků |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#dataset-properties) článku.

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud kopírujete z OData zdroje dat, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Příklad | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |"? $select = název, popis a $top = 5" |Ne |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#copy-activity-properties) článku.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine ODBC **typ** hello propojené služby příliš**OnPremisesOdbc**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| připojovací řetězec |šifrovat přihlašovací údaje bez přístupu část Hello hello připojovací řetězec a volitelné přihlašovacích údajů. Příklady v následující části hello. |Ano |
| přihlašovací údaje |Hello přístup pověření část hello připojovacího řetězce zadaného ve formátu ovladačem vlastnost hodnota. Příklad: "Uid =<user ID>; PWD =<password>; RefreshToken =<secret refresh token>; ". |Ne |
| authenticationType. |Typ ověřování používá úložiště dat rozhraní ODBC toohello tooconnect. Možné hodnoty jsou: anonymní a Basic. |Ano |
| uživatelské jméno |Pokud používáte základní ověřování, zadejte uživatelské jméno. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |
| gatewayName |Úložiště dat rozhraní ODBC toohello tooconnect měli použít název hello brány, kterou hello služba Data Factory. |Ano |

#### <a name="example---using-basic-authentication"></a>Příklad: použití základního ověřování

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Příklad: základní ověřování pomocí zašifrované přihlašovací údaje
Můžete šifrovat přihlašovací údaje hello pomocí hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny (1.0 verzi prostředí Azure PowerShell) nebo [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 nebo starší verzi hello Azure PowerShell).  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a>Příklad: Pomocí anonymní ověřování

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datovou sadu ODBC sadu hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v úložišti dat rozhraní ODBC hello. |Ano |


#### <a name="example"></a>Příklad

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#dataset-properties) článku. 

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud jsou kopírování dat z úložiště dat rozhraní ODBC, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Řetězec dotazu SQL. Například: `select * from MyTable`. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#copy-activity-properties) článku.

## <a name="salesforce"></a>Salesforce


### <a name="linked-service"></a>Propojená služba
toodefine Salesforce propojená služba, sada hello **typ** hello propojené služby příliš**Salesforce**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| environmentUrl | Zadejte adresu URL služby Salesforce instanci hello. <br><br> -Výchozí hodnota je "https://login.salesforce.com". <br> -toocopy data z izolovaného prostoru, zadejte "https://test.salesforce.com". <br> -toocopy data z vlastní doménu, zadejte například "https://[domain].my.salesforce.com". |Ne |
| uživatelské jméno |Zadejte uživatelské jméno pro hello uživatelský účet. |Ano |
| heslo |Zadejte heslo pro uživatelský účet hello. |Ano |
| securityToken |Zadejte token zabezpečení pro hello uživatelský účet. V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny tooreset nebo získat token zabezpečení. Obecně platí, najdete v části toolearn o tokeny zabezpečení [zabezpečení a hello rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine datovou sadu služby Salesforce, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| tableName |Název tabulky hello v Salesforce. |Ne (Pokud **dotazu** z **RelationalSource** je zadána) |

#### <a name="example"></a>Příklad

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#dataset-properties) článku. 

### <a name="relational-source-in-copy-activity"></a>Relačního zdroje v aktivitě kopírování
Pokud kopírujete data ze služby Salesforce, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| query |Použijte data tooread hello vlastního dotazu. |Dotaz SQL 92 nebo [Salesforce objektu dotazu jazyka (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) dotazu. Například `select * from MyTable__c`. |Ne (Pokud hello **tableName** z hello **datovou sadu** je zadána) |

#### <a name="example"></a>Příklad  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
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
        }]
    }
}
```

> [!IMPORTANT]
> Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.

Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#copy-activity-properties) článku. 

## <a name="web-data"></a>Data webové 

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine webové **typ** hello propojené služby příliš**webové**a zadejte následující vlastnosti v hello **rámci typeProperties** části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| URL |Adresa URL toohello webové zdroje |Ano |
| authenticationType. |Anonymní. |Ano |
 

#### <a name="example"></a>Příklad


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#linked-service-properties) článku. 

### <a name="dataset"></a>Datová sada
toodefine webové datové sady, sada hello **typ** sady dat hello příliš**WebTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části: 

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |Typ hello datovou sadu. musí být nastaven příliš**WebTable** |Ano |
| Cesta |Na relativní adresa URL toohello prostředek, který obsahuje tabulku hello. |Ne. Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby. |
| Index |index Hello hello tabulky v hello prostředku. V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části kroky toogetting index tabulky v stránku HTML. |Ano |

#### <a name="example"></a>Příklad

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#dataset-properties) článku. 

### <a name="web-source-in-copy-activity"></a>Webové zdroje v aktivitě kopírování
Pokud jsou kopírování dat z tabulky webové, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**WebSource**. V současné době po hello zdroj v aktivitě kopírování typu **WebSource**, jsou podporovány žádné další vlastnosti.

#### <a name="example"></a>Příklad

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
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
        }]
    }
}
```

Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#copy-activity-properties) článku. 

## <a name="compute-environments"></a>VÝPOČETNÍ PROSTŘEDÍ
Hello následující tabulka uvádí hello výpočetních prostředích nepodporuje objekt pro vytváření dat a hello aktivit transformace, které můžou běžet na ně. Klikněte na odkaz hello výpočetní hello se zajímá toosee hello JSON schémata pro propojenou službu toolink ho tooa data factory. 

| Výpočetní prostředí | Aktivity |
| --- | --- |
| [Cluster HDInsight na vyžádání](#on-demand-azure-hdinsight-cluster) nebo [vlastní cluster HDInsight](#existing-azure-hdinsight-cluster) |[Vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [činnost MapReduce](#hdinsight-mapreduce-activity), [streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity) |
| [Azure Batch](#azure-batch) |[Vlastní aktivita .NET](#net-custom-activity) |
| [Azure Machine Learning](#azure-machine-learning) | [Strojového učení aktivita provedení dávky](#machine-learning-batch-execution-activity), [strojového učení aktivita prostředku aktualizace](#machine-learning-update-resource-activity) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics) |[U-SQL Data Lake Analytics](#data-lake-analytics-u-sql-activity) |
| [Databáze Azure SQL](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [systému SQL Server](#sql-server-1) |[Uložená procedura](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>Cluster Azure HDInsight na vyžádání
Hello služby Azure Data Factory můžete automaticky vytvoří systému Windows nebo Linux na vyžádání HDInsight tooprocess data clusteru. Hello clusteru je vytvořen ve stejné oblasti jako účet úložiště hello (vlastnost linkedServiceName v hello JSON) přidruženého k hello clusteru hello. Můžete spustit následující aktivit transformace na tato propojená služba hello: [vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [činnost MapReduce ](#hdinsight-mapreduce-activity), [Streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Propojená služba 
Hello následující tabulka obsahuje popis vlastností hello použitých v definici Azure JSON hello na vyžádání propojené služby HDInsight.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavená příliš**HDInsightOnDemand**. |Ano |
| Parametr ClusterSize |Počet uzlů pracovního procesu nebo data v clusteru hello. Vytvoření clusteru HDInsight Hello s 2 hlavních uzlech společně s hello počet uzlů pracovního procesu, který jste zadali pro tuto vlastnost. Hello uzly jsou velikosti Standard_D3, který má 4 jádra, 4 pracovní uzly clusteru trvá 24 jader (4\*4 = 16 jader pro uzly pracovního procesu, plus 2\*4 = 8 jader pro head uzly). V tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) podrobnosti o hello Standard_D3 vrstvy. |Ano |
| TimeToLive |Hello povolená doba nečinnosti hello clusteru HDInsight na vyžádání. Určuje, jak dlouho clusteru HDInsight na vyžádání hello zůstane aktivní po dokončení činnosti spustit, pokud v clusteru hello nejsou žádné aktivní úlohy.<br/><br/>Pokud aktivita spuštění trvá 6 minut a timetolive je třeba nastavit too5 minut, hello clusteru zůstane aktivní po dobu 5 minut po hello 6 minut zpracování hello aktivity při spuštění. Pokud jiné aktivity při spuštění je proveden s hello 6 minut okna, je zpracován hello stejného clusteru.<br/><br/>Vytvoření clusteru HDInsight na vyžádání je náročná operace (může trvat), takže toto nastavení použijte jako potřebné tooimprove výkonu objektu pro vytváření dat pomocí opakovaného použití clusteru HDInsight na vyžádání.<br/><br/>Pokud jste nastavili too0 hodnota timetolive, odstranění clusteru hello co nejrychleji hello aktivita běžet v zpracovaná. Na hello druhé straně, pokud jste nastavili na vysokou hodnotu, hello clusteru může zůstat nečinnosti zbytečně výsledkem vysoké náklady. Proto je důležité nastavit hello odpovídající hodnotu na základě potřeb.<br/><br/>Více kanálů můžete sdílet stejnou instanci clusteru HDInsight na vyžádání hello text hello, pokud je správně nastavena hodnota vlastnosti timetolive hello |Ano |
| Verze |Verze clusteru HDInsight hello. Podrobnosti najdete v tématu [podporované verze HDInsight v Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). |Ne |
| linkedServiceName |Propojená služba toobe používá hello cluster na vyžádání pro ukládání a zpracování dat Azure Storage. <p>V současné době nelze vytvořit cluster HDInsight na vyžádání, která používá Azure Data Lake Store jako hello úložiště. Pokud chcete toostore hello Výsledná data z prostředí HDInsight zpracování v Azure Data Lake Store, pomocí aktivity kopírování toocopy hello dat z Azure Blob Storage toohello hello Azure Data Lake Store.</p>  | Ano |
| additionalLinkedServiceNames |Určuje, že další účty úložiště pro hello HDInsight propojená služba tak, aby služba Data Factory hello můžete zaregistrovat vaším jménem. |Ne |
| osType |Typ operačního systému. Povolené hodnoty jsou: systému Windows (výchozí) a Linux |Ne |
| hcatalogLinkedServiceName |Název Hello SQL Azure, propojené služby databázi HCatalog toohello bodu. cluster HDInsight na vyžádání Hello je vytvořená pomocí hello Azure SQL database jako hello metaúložiště. |Ne |

### <a name="json-example"></a>Příklad JSON
Hello následující JSON definuje základě Linux na vyžádání propojené služby HDInsight. Hello služba Data Factory automaticky vytvoří **systémem Linux** clusteru HDInsight se při zpracování dat řezu. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

Další informace najdete v tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) článku. 

## <a name="existing-azure-hdinsight-cluster"></a>Existující cluster Azure HDInsight
Tooregister Azure HDInsight propojené služby můžete vytvořit vlastní cluster HDInsight s Data Factory. Můžete spustit následující aktivit transformace dat na tato propojená služba hello: [vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [MapReduce aktivita](#hdinsight-mapreduce-activity), [streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Propojená služba
Hello následující tabulka obsahuje popis vlastností hello použitých v definici Azure JSON hello propojené služby Azure HDInsight.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavená příliš**HDInsight**. |Ano |
| clusterUri |identifikátor URI clusteru HDInsight hello Hello. |Ano |
| uživatelské jméno |Zadejte název hello toobe uživatele hello používá tooconnect tooan stávajícího clusteru HDInsight. |Ano |
| heslo |Zadejte heslo pro uživatelský účet hello. |Ano |
| linkedServiceName | Název hello Azure Storage, propojené služby, která odkazuje úložiště objektů blob Azure toohello používá hello clusteru HDInsight. <p>V současné době nelze zadat, že Azure Data Lake Store propojené služby pro tuto vlastnost. Může přistupovat k datům v Azure Data Lake Store hello z skriptů Hive nebo Pig Pokud hello HDInsight cluster má přístup toohello Data Lake Store. </p>  |Ano |

Verzích clusterů HDInsight, které jsou podporovány, naleznete v části [podporované HDInsight verze](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). 

#### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a>Azure Batch
Tooregister Azure Batch propojené služby můžete vytvořit pomocí služby data factory fondu služby Batch virtuálních počítačů (VM). Můžete spustit pomocí Azure Batch nebo Azure HDInsight vlastní aktivity .NET. Můžete spustit [vlastní aktivity .NET](#net-custom-activity) na toto propojenou službu. 

### <a name="linked-service"></a>Propojená služba
Hello následující tabulka obsahuje popis vlastností hello použitými v definici Azure JSON hello Azure Batch propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavená příliš**AzureBatch**. |Ano |
| název účtu |Název hello účtu Azure Batch. |Ano |
| accessKey |Přístupový klíč pro účet Azure Batch hello. |Ano |
| poolName |Název fondu hello virtuálních počítačů. |Ano |
| linkedServiceName |Název hello propojenou službu úložiště Azure přidruženého k této službě Azure Batch propojený. Tato propojená služba se používá pro pracovní soubory požadované toorun hello aktivity a ukládání hello protokoly provádění aktivity. |Ano |


#### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a>Azure Machine Learning
Vytvoření Azure Machine Learning propojené služby tooregister Machine Learning listu vyhodnocovací koncový bod pomocí služby data factory. Aktivity transformace dat dvě, které můžou běžet v této propojené službě: [aktivita provedení dávky Machine Learning](#machine-learning-batch-execution-activity), [aktivita prostředku aktualizace Machine Learning](#machine-learning-update-resource-activity). 

### <a name="linked-service"></a>Propojená služba
Hello následující tabulka obsahuje popis vlastností hello použitých v definici Azure JSON hello služby Azure Machine Learning propojený.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Typ |vlastnost typu Hello by měla být nastavena na: **AzureML**. |Ano |
| mlEndpoint |Hello dávkového vyhodnocování adresy URL. |Ano |
| apiKey |Hello publikovat rozhraní API modelu pracovního prostoru. |Ano |

#### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
Vytvoříte **Azure Data Lake Analytics** propojené služby toolink služby Azure Data Lake Analytics výpočetní služby tooan Azure data factory před použitím hello [aktivita Data Lake Analytics U-SQL](data-factory-usql-activity.md) v kanálu .

### <a name="linked-service"></a>Propojená služba

Hello následující tabulka obsahuje popis vlastností hello použitými v definici JSON hello Azure Data Lake Analytics propojené služby. 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Typ |vlastnost typu Hello by měla být nastavena na: **AzureDataLakeAnalytics**. |Ano |
| název účtu |Název účtu Azure Data Lake Analytics. |Ano |
| dataLakeAnalyticsUri |Identifikátor URI služby Azure Data Lake Analytics. |Ne |
| Autorizace |Autorizační kód se načte automaticky po kliknutí na **Autorizovat** tlačítka na hello editoru služby Data Factory a dokončuje přihlášení OAuth hello. |Ano |
| subscriptionId |Id předplatného Azure |Ne (když není určeno předplatné hello se používá pro vytváření dat). |
| Název skupiny prostředků |Název skupiny prostředků Azure. |Ne (když není určeno skupiny prostředků hello se používá pro vytváření dat). |
| ID relace |id relace z relace autorizace OAuth hello. Každé id relace je jedinečné a může být použit pouze jednou. Pokud používáte hello editoru služby Data Factory, toto ID se generuje automaticky. |Ano |


#### <a name="json-example"></a>Příklad JSON
Následující ukázka Hello poskytuje definici JSON pro služby Azure Data Lake Analytics propojený.

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a>Azure SQL Database
Vytvoření služby Azure SQL propojené a jeho použití s hello [aktivity uložené procedury](#stored-procedure-activity) tooinvoke uložené procedury z objektu pro vytváření dat kanál. 

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Azure SQL Database **typ** hello propojené služby příliš**azuresqldatabase**a zadejte následující vlastnosti v hello **rámci typeProperties**části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Database instance. |Ano |

#### <a name="json-example"></a>Příklad JSON

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

V tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Vytvoření služby Azure SQL Data Warehouse propojené a jeho použití s hello [aktivity uložené procedury](data-factory-stored-proc-activity.md) tooinvoke uložené procedury z objektu pro vytváření dat kanál. 

### <a name="linked-service"></a>Propojená služba
propojená služba, sada hello toodefine Azure SQL Data Warehouse **typ** hello propojené služby příliš**AzureSqlDW**a zadejte následující vlastnosti v hello **rámci typeProperties**části:  

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Data Warehouse instance. |Ano |

#### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku. 

## <a name="sql-server"></a>SQL Server 
Vytvoření služby SQL serveru propojená a jeho použití s hello [aktivity uložené procedury](data-factory-stored-proc-activity.md) tooinvoke uložené procedury z objektu pro vytváření dat kanál. 

### <a name="linked-service"></a>Propojená služba
Vytvoření propojené služby typu **onpremisessqlserver** toolink služby místní systém SQL Server databáze tooa data factory. Hello následující tabulka obsahuje popis služby SQL serveru propojená konkrétní tooon místní elementy JSON.

Hello následující tabulka obsahuje popis pro konkrétní tooSQL elementy JSON serveru propojené služby.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello by měla být nastavena na: **onpremisessqlserver**. |Ano |
| připojovací řetězec |Zadejte požadované informace connectionString tooconnect toohello místní databáze SQL serveru pomocí ověřování SQL nebo ověřování systému Windows. |Ano |
| gatewayName |Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi systému SQL Server. |Ano |
| uživatelské jméno |Zadejte uživatelské jméno, pokud používáte ověřování systému Windows. Příklad: **domainname\\uživatelské jméno**. |Ne |
| heslo |Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello. |Ne |

Můžete šifrovat přihlašovací údaje pomocí hello **New-AzureRmDataFactoryEncryptValue** rutiny a použijte je v hello připojovací řetězec, jak ukazuje následující příklad hello (**EncryptedCredential** vlastnost):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Příklad: JSON pro pomocí ověřování SQL.

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Příklad: JSON pro použití ověřování systému Windows

Pokud jsou zadané uživatelské jméno a heslo, brána používá, je tooimpersonate hello zadaný uživatelský účet tooconnect toohello místní databázi systému SQL Server. Jinak brána připojí toohello systému SQL Server přímo kontext zabezpečení hello brány (jeho účet spuštění).

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku.

## <a name="data-transformation-activities"></a>AKTIVITY TRANSFORMACE DAT

Aktivita | Popis
-------- | -----------
[Aktivitu HDInsight Hive](#hdinsight-hive-activity) | Hello aktivitu HDInsight Hive v kanálu pro vytváření dat provede dotazů Hive sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání. 
[Aktivita HDInsight Pig](#hdinsight-pig-activity) | Hello HDInsight Pig aktivity v kanálu pro vytváření dat provede Pig dotazy na vlastní nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.
[Aktivita MapReduce služby HDInsight](#hdinsight-mapreduce-activity) | Hello činnost HDInsight MapReduce v objektu pro vytváření dat kanál provede MapReduce programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.
[Aktivita Streamování služby HDInsight](#hdinsight-streaming-activity) | Hello HDInsight streamované aktivitě v objektu pro vytváření dat kanál provede streamování Hadoop programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.
[Aktivita Sparku služby HDInsight](#hdinsight-spark-activity) | Hello aktivity HDInsight Spark v objektu pro vytváření dat kanál provede na clusteru HDInsight Spark programy. 
[Aktivita Provedení dávky služby Machine Learning](#machine-learning-batch-execution-activity) | Azure Data Factory umožňuje tooeasily můžete vytvořit kanály, které používají publikované Azure Machine Learning webová služba pro prediktivní analýzy. Pomocí hello aktivita provedení dávky v kanál služby Azure Data Factory, můžete vyvolat Machine Learning webové služby toomake předpovědi hello data v dávce. 
[Aktivita Aktualizace prostředků služby Machine Learning](#machine-learning-update-resource-activity) | V průběhu času hello prediktivní modely v hello Machine Learning vyhodnocování experimenty potřebovat toobe retrained pomocí nové vstupní datové sady. Po dokončení práce s retraining, budete chtít tooupdate hello vyhodnocování webové služby s hello retrained model Machine Learning. Hello aktivita prostředku aktualizace tooupdate hello webové služby můžete použít s hello nově cvičení modelu.
[Aktivita Uložená procedura](#stored-procedure-activity) | Aktivity uložené procedury hello můžete použít v objektu pro vytváření dat kanál tooinvoke uložené procedury v jednom z hello následující úložišť dat: databáze SQL Azure, Azure SQL Data Warehouse, databáze SQL serveru ve vašem podniku nebo virtuální počítač Azure. 
[Aktivita data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL aktivity spouští skript U-SQL na clusteru služby Azure Data Lake Analytics.  
[Vlastní aktivita .NET](#net-custom-activity) | Pokud potřebujete tootransform data způsobem, který není podporován službou Data Factory, můžete vytvořit vlastní aktivity s vlastní logikou zpracování dat a používat hello aktivity v kanálu hello. Můžete nakonfigurovat hello vlastní .NET aktivity toorun pomocí služby Azure Batch nebo clusteru Azure HDInsight. 

     
## <a name="hdinsight-hive-activity"></a>Aktivita Hivu služby HDInsight
Můžete zadat následující vlastnosti v definici JSON aktivity Hive hello. musí být vlastnost typu Hello aktivity hello: **HDInsightHive**. Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightHive aktivity:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Skript |Zadejte vloženého skriptu Hive hello |Ne |
| cestu ke skriptu |Úložiště hello Hive skript v Azure blob storage a zadejte toohello hello cestě k souboru. Pomocí vlastnosti 'skript' nebo 'scriptPath'. Obě nelze použít společně. Název souboru Hello je malá a velká písmena. |Ne |
| definuje |Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skriptu Hive hello pomocí 'hiveconf. |Ne |

Tyto vlastnosti typu se konkrétní toohello Hive aktivity. Ostatní vlastnosti (mimo oddíl rámci typeProperties hello) jsou podporovány pro všechny aktivity.   

### <a name="json-example"></a>Příklad JSON
Hello následující JSON definuje aktivitu HDInsight Hive v kanálu.  

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

Další informace najdete v tématu [Hive aktivity](data-factory-hive-activity.md) článku. 

## <a name="hdinsight-pig-activity"></a>Aktivita Pig služby HDInsight
Můžete zadat následující vlastnosti v definici JSON aktivity Pig hello. musí být vlastnost typu Hello aktivity hello: **HDInsightPig**. Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightPig aktivity: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Skript |Zadejte vloženého skriptu Pig hello |Ne |
| cestu ke skriptu |Uložte skript Pig hello v Azure blob storage a zadejte toohello hello cestě k souboru. Pomocí vlastnosti 'skript' nebo 'scriptPath'. Obě nelze použít společně. Název souboru Hello je malá a velká písmena. |Ne |
| definuje |Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci hello skript Pig |Ne |

Tyto vlastnosti typu se konkrétní toohello Pig aktivity. Ostatní vlastnosti (mimo oddíl rámci typeProperties hello) jsou podporovány pro všechny aktivity.   

### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

Další informace najdete v tématu [Pig aktivity](#data-factory-pig-activity.md) článku. 

## <a name="hdinsight-mapreduce-activity"></a>Aktivita MapReduce služby HDInsight
Můžete zadat následující vlastnosti v definici JSON aktivity MapReduce hello. musí být vlastnost typu Hello aktivity hello: **HDInsightMapReduce**. Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightMapReduce aktivity: 

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jarLinkedService | Název hello propojené služby pro Azure Storage, který obsahuje soubor JAR hello hello. | Ano |
| jarFilePath | Cesta soubor JAR toohello v hello Azure Storage. | Ano | 
| Název třídy | Název hlavní třídy hello v soubor JAR hello. | Ano | 
| Argumenty | Seznam argumentů oddělených čárkami programu hello MapReduce. V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce hello. pomocí možnosti a hodnoty jako argumenty, jak ukazuje následující příklad hello toodifferentiate vaše argumenty s argumenty hello MapReduce, zvažte (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty) | Ne | 

### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix toodetermine hello similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce toogenerate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

Další informace najdete v tématu [činnost MapReduce](data-factory-map-reduce.md) článku. 

## <a name="hdinsight-streaming-activity"></a>Aktivita Streamování služby HDInsight
Můžete zadat následující vlastnosti v definici JSON aktivity streamování Hadoop hello. musí být vlastnost typu Hello aktivity hello: **HDInsightStreaming**. Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightStreaming aktivity: 

| Vlastnost | Popis | 
| --- | --- |
| Mapper | Název spustitelné hello mapper. V příkladu hello je cat.exe hello mapper spustitelný soubor.| 
| reduktorem | Název spustitelného souboru hello reduktorem. V příkladu hello je wc.exe hello reduktorem spustitelný soubor. | 
| Vstup | Vstupní soubor (včetně umístění) pro hello mapper. V příkladu hello: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample je kontejner objektů blob hello, například/data/Gutenberg je složka hello a davinci.txt je hello blob. |
| Výstup | Ve výstupním souboru (včetně umístění) reduktorem hello. výstup Hello úlohy streamování Hadoop hello je zapsán toohello umístění zadané pro tuto vlastnost. |
| filePaths | Cesty pro spustitelné soubory mapper a reduktorem hello. V příkladu hello: "adfsample/example/apps/wc.exe" adfsample je kontejner objektů blob hello, příklad nebo aplikací je složka hello a wc.exe je hello spustitelný soubor. | 
| fileLinkedService | Propojená služba, která představuje hello úložiště Azure, který obsahuje soubory hello zadané v části filePaths hello Azure Storage. | 
| Argumenty | Seznam argumentů oddělených čárkami programu hello MapReduce. V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce hello. pomocí možnosti a hodnoty jako argumenty, jak ukazuje následující příklad hello toodifferentiate vaše argumenty s argumenty hello MapReduce, zvažte (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty) | 
| getdebuginfo – | Element volitelné. Pokud je nastavené tooFailure, protokoly hello se stáhnou pouze při selhání. Pokud je nastavené tooAll, protokoly budou staženy vždy bez ohledu na stav spuštění hello. | 

> [!NOTE]
> Je nutné zadat výstupní datovou sadu pro hello streamované aktivitě Hadoop pro hello **výstupy** vlastnost. Tato datová sada může být právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán (hodinový, denní, atd.). Pokud aktivita hello neberou vstup, můžete přeskočit zadávání vstupní datové sady pro hello aktivitu pro hello **vstupy** vlastnost.  

## <a name="json-example"></a>Příklad JSON

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

Další informace najdete v tématu [streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md) článku. 

## <a name="hdinsight-spark-activity"></a>Aktivita Spark služby HDInsight
Můžete zadat následující vlastnosti v definici JSON aktivity Spark hello. musí být vlastnost typu Hello aktivity hello: **HDInsightSpark**. Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightSpark aktivity: 

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| rootPath | kontejner objektů Blob Azure Hello a složky, která obsahuje soubor Spark hello. Název souboru Hello je malá a velká písmena. | Ano |
| entryFilePath | Relativní cesta toohello kořenové složky hello Spark nebo balíček kódu. | Ano |
| Název třídy | Hlavní třídy aplikace Java/Spark | Ne | 
| Argumenty | Seznam argumentů příkazového řádku toohello Spark programu. | Ne | 
| proxyUser | Spark program hello tooimpersonate tooexecute Hello uživatelského účtu | Ne | 
| sparkConfig | Vlastnosti konfigurace Spark. | Ne | 
| getdebuginfo – | Určuje, když jsou soubory protokolu Spark hello zkopírovaný toohello úložiště Azure používá HDInsight cluster (nebo) zadaný ve sparkJobLinkedService. Povolené hodnoty: None, vždy nebo selhání. Výchozí hodnota: žádné. | Ne | 
| sparkJobLinkedService | Hello Azure Storage propojená služba, která obsahuje soubor úlohy Spark hello, závislosti a protokoly.  Pokud hodnotu pro tuto vlastnost nezadáte, použije se hello úložiště přidruženého k clusteru HDInsight. | Ne |

### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
Všimněte si hello následující body: 

- Hello **typ** vlastnost je nastavena příliš**HDInsightSpark**.
- Hello **rootPath** je nastaven příliš**adfspark\\pyFiles** kde adfspark je kontejner objektů Blob Azure hello a pyFiles je dobře složka v tomto kontejneru. V tomto příkladu je hello Azure Blob Storage hello ten, který je přidružen hello Spark cluster. Můžete nahrát soubor tooa hello jiného úložiště Azure. Pokud tak učiníte, vytváření Azure Storage, propojené služby toolink tohoto úložiště účet toohello dat. Potom zadejte název hello hello propojené služby jako hodnotu pro hello **sparkJobLinkedService** vlastnost. V tématu [vlastnosti aktivity Spark](#spark-activity-properties) podrobnosti o této vlastnosti a dalších vlastností nepodporuje hello Spark aktivity.
- Hello **entryFilePath** nastavena toohello **test.py**, což je soubor python hello. 
- Hello **getdebuginfo –** vlastnost je nastavena příliš**vždy**, což znamená, že soubory protokolu hello jsou vždy vygeneruje (úspěch nebo neúspěch).  

    > [!IMPORTANT]
    > Doporučujeme vám, nenastavujte tato vlastnost tooAlways v produkčním prostředí, pokud jsou řešení problémů. 
- Hello **výstupy** obsahuje jednu výstupní datovou sadu. Je třeba zadat datovou sadu výstupů i v případě, že hello spark program nevytváří žádný výstup. plán hello Hello výstupní datovou sadu jednotky pro kanál hello (hodinový, denní, atd.).

Další informace o aktivitě hello najdete v tématu [Spark aktivity](data-factory-spark.md) článku.  

## <a name="machine-learning-batch-execution-activity"></a>Aktivita Provedení dávky služby Machine Learning
Můžete zadat následující vlastnosti v definici Azure ML dávky spuštění aktivity JSON hello. musí být vlastnost typu Hello aktivity hello: **AzureMLBatchExecution**. Musíte vytvořit Azure Machine Learning nejprve propojené služby a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooAzureMLBatchExecution aktivity:

Vlastnost | Popis | Požaduje se 
-------- | ----------- | --------
webServiceInput | datovou sadu toobe Hello předat jako vstup pro hello Azure ML webové služby. Tato datová sada musí být součástí hello vstupy aktivity hello. |Použijte webServiceInput nebo webServiceInputs. | 
webServiceInputs | Zadejte, datové sady toobe předány jako vstupy pro hello Azure ML webové služby. Pokud hello webová služba přijímá více vstupů, použijte vlastnost webServiceInputs hello místo použití hello webServiceInput vlastnost. Datové sady, které odkazují hello **webServiceInputs** musí také obsahovat hello aktivity **vstupy**. | Použijte webServiceInput nebo webServiceInputs. | 
webServiceOutputs | Hello datové sady, který je přiřazen jako výstup pro webovou službu Azure ML hello. Webová služba Hello vrátí výstupní data v této datové sadě. | Ano | 
globalParameters | Zadejte hodnoty pro parametry hello webové služby v této části. | Ne | 

### <a name="json-example"></a>Příklad JSON
V tomto příkladu hello aktivita má datovou sadu hello **MLSqlInput** jako vstup a **MLSqlOutput** jako výstup hello. Hello **MLSqlInput** se předá jako webové služby vstupní toohello podle pomocí hello **webServiceInput** vlastnost JSON. Hello **MLSqlOutput** se předá jako výstup toohello webové služby s použitím hello **webServiceOutputs** vlastnost JSON. 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

V příkladu JSON hello hello nasadili Azure Machine Learning webové služby používá čtečka čipových karet a zapisovače modulu tooread a zápis dat z / tooan Azure SQL Database. Tato webová služba zpřístupní hello následující čtyři parametry: název serveru, název databáze, název serveru uživatelského účtu a heslo uživatelského účtu serveru databáze.

> [!NOTE]
> Pouze vstupy a výstupy hello AzureMLBatchExecution aktivity mohou být předána jako toohello parametry webové služby. Například v hello výše fragmentu kódu JSON, je MLSqlInput vstupní toohello AzureMLBatchExecution aktivity, které se předá jako vstupní toohello webové služby pomocí parametru webServiceInput.

## <a name="machine-learning-update-resource-activity"></a>Aktivita aktualizace prostředku služby Machine Learning
Můžete zadat následující vlastnosti v definici Azure ML aktualizace prostředků aktivity JSON hello. musí být vlastnost typu Hello aktivity hello: **AzureMLUpdateResource**. Musíte vytvořit Azure Machine Learning nejprve propojené služby a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooAzureMLUpdateResource aktivity:

Vlastnost | Popis | Požaduje se 
-------- | ----------- | --------
trainedModelName | Název hello retrained modelu. | Ano |  
trainedModelDatasetName | Datová sada polohovací toohello reprezentuje soubor iLearner vrácený hello retraining operaci. | Ano | 

### <a name="json-example"></a>Příklad JSON
Hello kanálu má dvě aktivity: **AzureMLBatchExecution** a **AzureMLUpdateResource**. Hello aktivita provedení dávky Azure ML trvá hello Cvičná data jako vstup a vytvoří soubor iLearner jako výstup. Aktivita Hello vyvolá hello školení webové služby (výukový experiment zveřejněné jako webovou službu) se vstupem hello trénovací data a obdrží od hello webservice soubor ilearner hello. Hello placeholderBlob je právě fiktivní výstupní datovou sadu, která vyžadují hello Azure Data Factory služby toorun hello kanálu.


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a>Aktivita U-SQL služby Data Lake Analytics
Můžete zadat hello následující vlastnosti v definici JSON aktivity U-SQL. musí být vlastnost typu Hello aktivity hello: **DataLakeAnalyticsU SQL**. Musíte vytvořit služby Azure Data Lake Analytics propojené a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ aktivity tooDataLakeAnalyticsU-SQL: 

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| scriptPath |Cesta toofolder, který obsahuje skript hello U-SQL. Název souboru hello rozlišuje velká a malá písmena. |Ne (když používáte skript) |
| scriptLinkedService |Propojené služby, který odkazuje hello úložiště, který obsahuje toohello hello skriptu pro vytváření dat |Ne (když používáte skript) |
| Skript |Zadejte místo zadání scriptPath a scriptLinkedService zpracování vloženého skriptu. Například: "skript": "Vytvořit databázi test". |Ne (když používáte scriptPath a scriptLinkedService) |
| degreeOfParallelism |maximální počet uzlů Hello současně použít toorun hello úlohy. |Ne |
| Priorita |Určuje, které z uložených ve frontě úloh by měl být vybrané toorun nejdřív. Hello nižší hello číslo, vyšší prioritu hello hello. |Ne |
| parameters |Parametry pro skript hello U-SQL |Ne |

### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

Další informace najdete v tématu [Data Lake Analytics U-SQL aktivity](data-factory-usql-activity.md). 

## <a name="stored-procedure-activity"></a>Aktivita Uložená procedura
Můžete zadat následující vlastnosti v definici uložené procedury aktivity JSON hello. musí být vlastnost typu Hello aktivity hello: **SqlServerStoredProcedure**. Musíte vytvořit jednu z hello následující propojené služby a zadejte název hello hello propojené služby jako hodnotu pro hello **linkedServiceName** vlastnost:

- SQL Server 
- Azure SQL Database
- Azure SQL Data Warehouse

Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooSqlServerStoredProcedure aktivity:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| storedProcedureName |Zadejte název hello hello uložené procedury v hello Azure SQL database nebo Azure SQL Data Warehouse, která je reprezentována hello propojené služby, která hello používá výstupní tabulka. |Ano |
| storedProcedureParameters |Zadejte hodnoty pro parametry uložené procedury. Pokud potřebujete toopass hodnotu null pro parametr, použijte syntaxi hello: "param1": null (všechny malá písmena). Viz následující ukázka toolearn o používání této vlastnosti hello. |Ne |

Pokud zadáte vstupní datové sady, musí být k dispozici (v 'Připravený' stav) pro hello uložené procedury aktivity toorun. vstupní datové sady Hello nelze zpracovat v hello uložené procedury jako parametr. Je pouze použité toocheck hello závislostí před počáteční hello uložené procedury aktivity. Je nutné zadat výstupní datovou sadu aktivity uložené procedury. 

Výstupní datové sady určuje hello **plán** pro hello uložené procedury aktivity (každou hodinu, týdně, měsíčně, atd.). Hello výstupní datovou sadu musí používat **propojená služba** který odkazuje tooan databáze SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, ve kterém chcete hello toorun uložené procedury. Hello výstupní datovou sadu může sloužit jako výsledek hello toopass způsob hello uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) v kanálu hello. Ale objekt pro vytváření dat nelze zapsat automaticky hello výstupní datové sady toothis uložené procedury. Je, že hello této tabulky SQL tooa zápisů, které hello výstupní datová sada odkazuje na uložené procedury. V některých případech může být hello výstupní datovou sadu **fiktivní datovou sadu**, který se používá jen toospecify hello plánu pro spuštěnou hello uložené procedury aktivity.  

### <a name="json-example"></a>Příklad JSON

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

Další informace najdete v tématu [aktivity uložené procedury](data-factory-stored-proc-activity.md) článku. 

## <a name="net-custom-activity"></a>Vlastní aktivita .NET
Můžete zadat následující vlastnosti v rozhraní .NET vlastní aktivity definici JSON hello. musí být vlastnost typu Hello aktivity hello: **DotNetActivity**. Musíte vytvořit propojené služby Azure HDInsight nebo Azure Batch propojené služby a zadejte název hello hello propojené služby jako hodnotu pro hello **linkedServiceName** vlastnost. Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooDotNetActivity aktivity:
 
| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| AssemblyName | Název sestavení hello. V příkladu hello je: **MyDotnetActivity.dll**. | Ano |
| Vstupní bod |Název třídy hello, který implementuje rozhraní IDotNetActivity hello. V příkladu hello je: **MyDotNetActivityNS.MyDotNetActivity** kde MyDotNetActivityNS je obor názvů hello a MyDotNetActivity je třída hello.  | Ano | 
| PackageLinkedService | Název propojené služby Azure Storage ukazující toohello úložiště objektů blob, který obsahuje soubor zip vlastní aktivity hello hello. V příkladu hello je: **AzureStorageLinkedService**.| Ano |
| PackageFile | Název souboru zip hello. V příkladu hello je: **customactivitycontainer/MyDotNetActivity.zip**. | Ano |
| ExtendedProperties | Rozšířené vlastnosti, které můžete definovat a předat na kód toohello .NET. V tomto příkladu hello **SliceStart** proměnná je nastavená hodnota tooa podle hello SliceStart systémové proměnné. | Ne | 

### <a name="json-example"></a>Příklad JSON

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

Podrobné informace najdete v tématu [použít vlastní aktivity v datové továrně](data-factory-use-custom-activities.md) článku. 

## <a name="next-steps"></a>Další kroky
V tématu hello následující kurzy: 

- [Kurz: vytvoření kanálu s aktivitou kopírování](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Kurz: vytvoření kanálu s aktivitou hive](data-factory-build-your-first-pipeline-using-editor.md)