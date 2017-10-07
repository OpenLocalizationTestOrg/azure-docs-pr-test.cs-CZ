---
title: "aaaScheduling a spuštění pomocí služby Data Factory | Microsoft Docs"
description: "Další aspekty plánování a provádění aplikačního modelu služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 6114dd4896f5537c789c3b632fb90e501b694285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-factory-scheduling-and-execution"></a>Data Factory plánování a provádění
Tento článek vysvětluje aspekty plánování a provádění hello hello Azure Data Factory aplikačního modelu služby. Tento článek předpokládá, že chápete základní informace o objektu pro vytváření dat aplikací modelu koncepty, včetně aktivit, kanálů, propojené služby a datové sady. Základní koncepty objektu pro vytváření dat Azure najdete v části hello následující články:

* [Úvod tooData objekt pro vytváření](data-factory-introduction.md)
* [Kanály](data-factory-create-pipelines.md)
* [Datové sady](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>Počáteční a koncový čas kanálu
Kanál je aktivní jenom mezi jeho **spustit** čas a **end** čas. Nebude provedena před časem zahájení hello nebo po hello koncový čas. Pokud hello kanálu pozastaví, nebude provedena bez ohledu na jeho počáteční a koncový čas. Pro toorun kanálu by nemělo být pozastavena. Zjistíte, tato nastavení (zahájení, ukončení, pozastavena) v definici kanálu hello: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

Další informace najdete v těchto vlastností [vytvořit kanály](data-factory-create-pipelines.md) článku. 


## <a name="specify-schedule-for-an-activity"></a>Zadejte plán pro aktivitu
Není hello kanál, který se spustí. Je hello aktivity v kanálu hello, které jsou spouštěny v hello celkové kontextu hello kanálu. Můžete zadat plán opakování pro aktivitu pomocí hello **Plánovač** část JSON aktivity. Například můžete naplánovat aktivitu toorun každou hodinu následujícím způsobem:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

Jak ukazuje následující diagram hello, zadat že plán pro aktivitu vytvoří řadu přeskakující windows se v hello kanálu spuštění a ukončení. Přeskakující windows jsou řadu pevné velikosti nepřekrývají, souvislý časové intervaly. Tyto logické přeskakující windows pro aktivitu se nazývají **aktivity windows**.

![Příklad aktivitu plánovače](media/data-factory-scheduling-and-execution/scheduler-example.png)

Hello **Plánovač** vlastnost pro aktivitu je nepovinná. Pokud určíte tuto vlastnost, musí se shodovat hello cadence, který určíte v definici hello výstupní datovou sadu aktivity hello. Výstupní datové sady v současné době je, jaké jednotky hello plán. Proto je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup. 

## <a name="specify-schedule-for-a-dataset"></a>Zadejte plán pro datové sady
Aktivita v kanálu pro vytváření dat může trvat vstup nula nebo více **datové sady** a vytvoří výstupní datové sady. Pro aktivitu, můžete zadat hello cadence, na které hello je k dispozici vstupní data nebo data výstup hello je vytvořená pomocí hello **dostupnosti** část v definicích hello datovou sadu. 

**Frekvence** v hello **dostupnosti** část určuje hello časovou jednotku. Hello povolené jsou hodnoty frekvence: minutu, hodinu, den, týden a měsíce. Hello **interval** vlastnost v části dostupnosti hello určuje multiplikátor pro četnost. Například: Pokud je nastavena hello frekvence tooDay a interval se nastavuje too1 pro datovou sadu výstupů, hello výstupní data se vytvoří každý den. Pokud zadáte četnost hello jako minutu, doporučujeme, abyste nastavili hello interval toono méně než 15. 

V následujícím příkladu hello, hello vstupních dat je k dispozici každou hodinu a výstupní data hello se vytvářejí každou hodinu (`"frequency": "Hour", "interval": 1`). 

**Vstupní datové sady:** 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```


**Výstupní datové sady**

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

V současné době **výstupní datovou sadu jednotky hello plán**. Jinými slovy zadaný pro výstupní datovou sadu hello plán hello je použité toorun aktivitu za běhu. Proto je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup. Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello. 

V následující hello kanálu definice hello **Plánovač** vlastnost je použité toospecify plán aktivity hello. Tato vlastnost je nepovinná. V současné době hello plán aktivity hello musí odpovídat hello plán zadaný pro hello výstupní datovou sadu.
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
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
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

V tomto příkladu hello běh aktivit každou hodinu mezi hello spuštění a ukončení hello kanálu. Hello výstupních dat každou hodinu vytváří pro windows 3 hodiny (8: 00 - 9 AM, 9: 00 – 10: 00 a 10 AM - 11 AM). 

Je volána jednotlivých jednotek data využívat nebo vyprodukované aktivitu spustit **datový řez**. Hello následující diagram ukazuje příklad aktivitu jednu vstupní datovou sadu a jednu výstupní datovou sadu: 

![Dostupnost plánovače](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Hello diagram znázorňuje hello hodinové datové řezy pro hello vstupní a výstupní datovou sadu. Hello diagram znázorňuje tři vstupní řezy, které jsou připravené ke zpracování. Aktivita 10 11 AM Hello je v průběhu vytváření hello 10 11 AM výstupní řez. 

Dostanete hello časový interval přidružené hello aktuálního řezu v datové sadě hello JSON pomocí proměnných: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) a [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables). Podobně můžete přistupovat přidružené okno s aktivity pomocí hello WindowStart a WindowEnd hello časový interval. plán Hello aktivity musí odpovídat hello plán hello výstupní datovou sadu aktivity hello. Proto hello SliceStart a SliceEnd hodnoty jsou hello stejné jako WindowStart a WindowEnd hodnoty v uvedeném pořadí. Další informace o těchto proměnných najdete v tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md#data-factory-system-variables) články.  

Tyto proměnné můžete použít pro jiné účely vaše aktivity JSON. Například můžete jejich data tooselect ze vstupní a výstupní datové sady reprezentující data časové řady (například: 8 AM too9 m). Tento příklad také používá **WindowStart** a **WindowEnd** tooselect relevantní data pro aktivitu spustit a zkopírujte ho tooa objektů blob s hello odpovídající **folderPath**. Hello **folderPath** je parametrizované toohave samostatnou složku pro každou hodinu.  

V předchozím příkladu hello je zadaný pro vstupní a výstupní datové sady plán hello hello stejné (každou hodinu). Pokud vstupní datové sady hello aktivity hello je k dispozici na jinou frekvenci, například každých 15 minut, hello aktivity, která vytváří tento výstupní datovou sadu stále spouští jednou za hodinu hello výstupní datová sada, jaké jednotky hello plán aktivity. Další informace najdete v tématu [modelu datové sady s jinou frekvencí](#model-datasets-with-different-frequencies).

## <a name="dataset-availability-and-policies"></a>Dostupnost datové sady a zásady
Jste viděli hello využití frekvence a intervalu vlastností v části dostupnosti hello definice datové sady. Existuje několik vlastností, které ovlivňují hello plánování a provádění aktivity. 

### <a name="dataset-availability"></a>Datovou sadu dostupnosti 
Hello následující tabulka popisuje vlastnosti, které můžete použít v hello **dostupnosti** části:

| Vlastnost | Popis | Požaduje se | Výchozí |
| --- | --- | --- | --- |
| frequency |Určuje časovou jednotku hello k produkci řez datovou sadu.<br/><br/><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc |Ano |Není k dispozici |
| interval |Určuje multiplikátor pro četnost<br/><br/>"Frekvence x interval" Určuje, jak často hello se vytvářejí.<br/><br/>Pokud třeba hello datovou sadu toobe rozříznut hodinu, nastavíte <b>frekvence</b> příliš<b>hodinu</b>, a <b>interval</b> příliš<b>1</b>.<br/><br/><b>Poznámka:</b>: Pokud zadáte četnost jako minutu, doporučujeme, abyste nastavili hello interval toono méně než 15 |Ano |Není k dispozici |
| Styl |Určuje, zda by měl být na hello počáteční nebo koncové intervalu hello předložen hello řez.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Pokud je nastavena frekvence tooMonth a je nastaven styl tooEndOfInterval, hello se vytvářejí na hello poslední den v měsíci. Pokud je styl hello nastavená tooStartOfInterval, hello se vytvářejí na hello první den v měsíci.<br/><br/>Pokud je nastavena frekvence tooDay a je nastaven styl tooEndOfInterval, hello se vytvářejí v hello poslední hodiny dne hello.<br/><br/>Pokud je nastavena frekvence tooHour a je nastaven styl tooEndOfInterval, hello se vytvářejí na konci hello hello hodina. Například pro řez dobu 13: 00 – 14: 00, hello se vytvářejí na 14: 00. |Ne |EndOfInterval |
| anchorDateTime |Definuje hello absolutní pozici v čase, které používají scheduler toocompute datovou sadu řez hranic. <br/><br/><b>Poznámka:</b>: Pokud má hello AnchorDateTime částí data, která jsou podrobnější než frekvence hello pak hello podrobnější části jsou ignorovány. <br/><br/>Například, pokud hello <b>interval</b> je <b>každou hodinu</b> (frekvence: hodin a interval: 1) a hello <b>AnchorDateTime</b> obsahuje <b>minuty a sekundy</b>, pak hello <b>minuty a sekundy</b> částí hello AnchorDateTime jsou ignorovány. |Ne |01/01/0001 |
| Posun |Časový interval, ve které hello začátku a konci všech řezech datovou sadu posunuty. <br/><br/><b>Poznámka:</b>: Pokud jsou zadané anchorDateTime i posun, výsledkem hello je hello kombinaci shift. |Ne |Není k dispozici |

### <a name="offset-example"></a>Příklad posunutí
Ve výchozím nastavení, každý den (`"frequency": "Day", "interval": 1`) řezy spuštění na čas UTC 12: 00 (půlnoc). Pokud chcete místo toho hello počáteční čas čas UTC toobe 6: 00, nastavte hello posun, jak je znázorněno v následujícím fragmentu kódu hello: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>Příklad anchorDateTime
V následujícím příkladu hello hello sada vytváří jednou za 23 hodin. Hello první řez spustí v době hello určeného hello anchorDateTime, který je nastaven příliš`2017-04-19T08:00:00` (Světový čas UTC).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>Posun nebo styl příklad
Hello následující datové sady je měsíční datová sada a vytváří 3rd v každém měsíci v 8:00 AM (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>Datovou sadu zásad
Datovou sadu, může mít definované zásady ověřování, která určuje, jak může být ověřen hello data generována řez provádění předtím, než je připraven ke spotřebování. V takových případech po dokončení provádění hello řez hello výstupní řez je změnit stav příliš**čekání** s substatus z **ověření**. Po ověření hello řezy jsou stav řezu hello změní příliš**připraven**. Pokud datový řez pochází, ale neprošel ověřením platnosti hello, nebudou zpracovány spuštění aktivity pro příjem dat datové řezy, které závisí na tento řez. [Monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) zahrnuje hello různé stavy datové řezy ve službě Data Factory.

Hello **zásad** oddíl v definici datové sady definuje kritéria hello nebo hello podmínku, která hello řezy datovou sadu musí splnit. Hello následující tabulka popisuje vlastnosti, které můžete použít v hello **zásad** části:

| Název zásady | Popis | Použít příliš| Požaduje se | Výchozí |
| --- | --- | --- | --- | --- |
| minimumSizeMB | Ověří, zda hello data ve **objektů blob v Azure** hello splňuje požadavky na minimální velikost (v megabajtech). |Azure Blob |Ne |Není k dispozici |
| minimumRows | Ověří, zda hello data ve **Azure SQL database** nebo **tabulky Azure** obsahuje hello minimální počet řádků. |<ul><li>Azure SQL Database</li><li>Tabulky Azure</li></ul> |Ne |Není k dispozici |

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

**minimumRows**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

Další informace o těchto vlastnostech a příklady naleznete v tématu [vytvoření datových sad](data-factory-create-datasets.md) článku. 

## <a name="activity-policies"></a>Zásady aktivit
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

Další informace najdete v tématu [kanály](data-factory-create-pipelines.md) článku. 

## <a name="parallel-processing-of-data-slices"></a>Paralelní zpracování datové řezy
Hello počáteční datum pro kanál hello můžete nastavit v posledních hello. Pokud tak učiníte, Data Factory automaticky vypočítá všechny datové řezy v posledních hello (back výplněmi) a zahájí zpracování je. Například: Pokud vytvoření kanálu s počátečním datem 2017-04-01 a hello aktuální datum následuje 2017-04-10. Pokud výstup hello cadence Dobrý den, že se datová sada denně a potom spustí služby Data Factory zpracování všech řezech hello z 2017-04-01 too2017-04-09 okamžitě protože hello počáteční datum je v minulosti hello. Hello z 2017-04-10 není zpracování řezu ještě protože hello vlastnost stylu v části dostupnosti hello hodnotu EndOfInterval ve výchozím nastavení. Hello nejstarší zpracování řezu se nejprve jako výchozí hello executionPriorityOrder hodnotu OldestFirst. Popis vlastnost stylu hello najdete v tématu [datovou sadu dostupnosti](#dataset-availability) části. Popis části executionPriorityOrder hello najdete v tématu hello [zásady aktivit](#activity-policies) části. 

Můžete nakonfigurovat toobe řezy vyplněno zpět data souběžně zpracovávaných nastavení hello **souběžnosti** vlastnost hello **zásad** část JSON hello aktivity. Tato vlastnost určuje hello počet spuštěních paralelní aktivity, které se může stát při jiné řezy. Hello výchozí hodnota pro vlastnost souběžnosti hello je 1. Proto jeden zpracování řezu se současně ve výchozím nastavení. Hello maximální hodnota je 10. Když kanál potřebuje toogo prostřednictvím velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat hello. 

## <a name="rerun-a-failed-data-slice"></a>Opětovné spuštění neúspěšné datový řez
Když dojde k chybě při zpracování dat řezu, můžete zjistit proč pomocí oken webu Azure portal nebo monitorování a správě aplikace hello zpracování řezu se nezdařilo. V tématu [monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md) nebo [monitorování a správu aplikace](data-factory-monitor-manage-app.md) podrobnosti.

Vezměte v úvahu následující příklad, který ukazuje dvě aktivity hello. Aktivity "activity1" a aktivitu 2. Aktivity "activity1" využívá řez Dataset1 a produkuje řez Dataset2, které se spotřebovávají "activity2" tooproduce řez hello poslední datovou sadu jako vstup.

![Řez se nezdařilo](./media/data-factory-scheduling-and-execution/failed-slice.png)

Hello diagram ukazuje, že mimo tři poslední řezů, že došlo 9 10 AM řez vytváření hello selhání pro Dataset2. Objekt pro vytváření dat automaticky sleduje závislostí pro datovou sadu řady čas hello. V důsledku toho se nespustí hello aktivity při spuštění pro příjem dat řez hello AM 9-10.

Datový objekt pro vytváření monitorování a nástroje pro správu umožní toodrill do diagnostickým protokolům hello hello selhání řez tooeasily najít hello kořenové způsobit hello problému a opravte ho. Po opravení problému hello můžete snadno začít hello aktivity při spuštění se nezdařilo řez tooproduce hello. Další informace o tom, toorerun a pochopit přechodů mezi stavy pro datové řezy, najdete v části [monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md) nebo [monitorování a správu aplikace](data-factory-monitor-manage-app.md).

Po znovu hello 9 10 AM slice pro **Dataset2**, spustí hello spustit pro hello 9 10 AM závislé řez na hello poslední datovou sadu služby Data Factory.

![Opětovné spuštění neúspěšné řez](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Více aktivit v kanálu
Kanál může obsahovat víc než jednu aktivitu. Pokud máte více aktivit v kanálu a hello výstup aktivity není vstup jinou aktivitu, může paralelně spustit hello aktivity, pokud připravení řezy vstupní data aktivity hello.

Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Hello aktivity mohou být v hello stejné kanálu nebo v jiné kanály. Druhá aktivita Hello spustí jenom v případě, že hello nejdřív jednu příště úspěšně dokončena.

Představte si třeba hello následující případ, kdy kanálu má dvě aktivity:

1. A1 aktivity, která vyžaduje externí vstupní datové sady D1 a vytvoří výstupní datovou sadu D2.
2. A2 aktivity, který vyžaduje vstup z datové sady D2 a vytvoří výstupní datovou sadu D3.

Tento scénář, aktivity A1 a A2 jsou v hello stejné kanálu. Hello aktivity A1 spustí hello externích dat je k dispozici a je dosaženo hello Naplánované dostupnosti frekvence. Hello aktivity A2 spustí hello naplánované řezy z D2 k dispozici a hello naplánovaná dostupnost frekvence je dosaženo. Pokud dojde k chybě v jednom z hello řezy v datové sadě D2, nespustí se pro tento řez A2, dokud nebude k dispozici.

Hello zobrazení diagramu s obou aktivitami v hello, že stejné kanálu bude vypadat hello následující diagram:

![Řetězení aktivity v hello stejné kanálu](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

Jak už bylo zmíněno dříve, hello aktivity může být v jiné kanály. V takové situaci zobrazení diagramu hello bude vypadat hello následující diagram:

![Řetězení aktivity ve dvou kanálů](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

V tématu hello [zkopírujte postupně](#copy-sequentially) část v příloze hello příklad.

## <a name="model-datasets-with-different-frequencies"></a>Datové sady modelu s jinou frekvence
V hello ukázky byly hello stejné hello frekvencí pro vstupní a výstupní datové sady a hello aktivity okno plánu. Některé scénáře vyžadují hello možnost tooproduce výstup frekvencí, který se liší od hello frekvence jeden nebo více vstupů. Objekt pro vytváření dat podporuje tyto scénáře modelování.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Příklad 1: Vytvoření sestavy denní výstup pro vstupní data, která je k dispozici každou hodinu
Vezměte v úvahu scénář, ve kterém můžete mít vstupní měření data ze senzorů, které jsou k dispozici každou hodinu do úložiště objektů Blob v Azure. Chcete, aby tooproduce denní agregační sestava s statistické údaje, třeba střední, maximální a minimální hello den s [Data Factory hive aktivity](data-factory-hive-activity.md).

Zde je, jak můžete model tento scénář se objekt pro vytváření dat:

**Vstupní datové sady**

Hello vstupní každou hodinu zahozených soubory ve složce hello pro hello zadaný den. Dostupnost pro vstup je nastavený na **hodinu** (frekvence: hodiny, interval: 1).

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Výstupní datové sady**

Jedna výstupní soubor se vytvoří každý den ve složce hello den. Dostupnost výstupu je nastavený na **den** (frekvence: den a interval: 1).

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Aktivity: aktivitu v kanálu hivu**

skript hive Hello obdrží hello odpovídající *data a času* informace jako parametry, které používají hello **WindowStart** proměnné, jak je znázorněno v následujícím fragmentu kódu hello. skript hive Hello používá tato data hello proměnné tooload z správnou složku hello hello den a spusťte hello agregace toogenerate hello výstup.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
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
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:MM}',WindowStart)",
                    "Day": "$$Text.Format('{0:dd}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

Hello následující diagram znázorňuje hello scénář z hlediska data závislostí.

![Data závislostí](./media/data-factory-scheduling-and-execution/data-dependency.png)

Hello výstupní řez pro každý den, závisí na 24 řezů hodinové ze vstupní datové sady. Data Factory výpočtů tyto závislosti automaticky po zjištění hello vstupní datové řezy, které se nacházejí v hello stejné jako hello výstupní řez toobe vytvořeného časové období. Pokud žádné vstupní řezy hello 24 není k dispozici, čeká na objekt pro vytváření dat toobe vstupní řez hello připravené před spuštěním hello denní aktivity při spuštění.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Příklad 2: Zadejte závislostí s výrazy a funkce pro vytváření dat
Pojďme se jiný scénář. Předpokládejme, že máte aktivitu hive, který zpracovává dvě vstupní datové sady. Jeden z nich má nová data denně, ale jeden z nich získá nová data každý týden. Předpokládejme, že jste chtěli toodo spojení mezi dvěma vstupy hello a vytvořit výstup každý den.

Hello jednoduchý přístup, ve kterém Data Factory automaticky hodnoty na pravém vstup hello řezy tooprocess slaďte toohello výstup, který datový řez časové období není funkční.

Je nutné zadat, že pro každé aktivity při spuštění, hello objekt pro vytváření dat mají používat minulého týdne datový řez hello týdenní vstupní datové sady. Můžete použít funkce Azure Data Factory, jak je znázorněno v následujícím fragmentu kódu tooimplement hello toto chování.

**Input1: Azure blob**

Hello první vstup, se právě hello objektů blob v Azure aktualizuje denně.

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Input2: Azure blob**

Input2 je hello objektů blob v Azure aktualizují každý týden.

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

**Výstup: Azure blob**

Jedna výstupní soubor se vytvoří každý den ve složce hello hello den. Dostupnost výstupu je nastaven příliš**den** (frekvence: den, interval: 1).

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Aktivity: aktivitu v kanálu hivu**

Aktivita hive Hello trvá hello dva vstupy a produkuje řez výstupních každý den. Můžete zadat každý den výstupní řez toodepend na dobrý den předchozího týdne vstupní řez pro týdenní vstup následujícím způsobem.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:MM}',WindowStart)",
            "Day": "$$Text.Format('{0:dd}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

V tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md) seznam funkce a systémové proměnné, které podporuje služby Data Factory.

## <a name="appendix"></a>Příloha

### <a name="example-copy-sequentially"></a>Příklad: kopírování postupně
Ho je možné toorun více operací kopírování jedna po druhé způsobem sekvenční/řazení. Například můžete mít dvě kopie aktivity v kanálu (CopyActivity1 a CopyActivity2) s hello následující vstupní data výstupní datové sady:   

CopyActivity1

Vstup: datové sady. Výstup: Dataset2.

CopyActivity2

Vstup: Dataset2.  Výstup: Dataset3.

CopyActivity2 spustí jenom v případě hello CopyActivity1 proběhla úspěšně a Dataset2 je k dispozici.

Tady je ukázkový kanál služby hello JSON:

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

Všimněte si, že v příkladu hello hello výstupní datovou sadu hello první aktivitu kopírování (Dataset2) zadané jako vstup pro druhá aktivita hello. Proto hello druhá aktivita spustí pouze v případě, že hello výstupní datové sady z první aktivitu hello je připraven.  

V příkladu hello CopyActivity2 může mít různé vstupu, například Dataset3, ale zadat Dataset2 jako vstupní tooCopyActivity2, takže hello aktivity nespustí, dokud nebude dokončeno CopyActivity1. Například:

CopyActivity1

Vstup: Dataset1. Výstup: Dataset2.

CopyActivity2

Vstupy: Dataset3, Dataset2. Výstup: Dataset4.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

Všimněte si, že v příkladu hello jsou dva vstupní datové sady zadané pro aktivitu kopírování druhý hello. Jsou-li více vstupů, pouze hello první vstupní datové sady se používá ke kopírování dat, ale jiné datové sady se používají jako závislosti. CopyActivity2 by spustit až po hello jsou splněny následující podmínky:

* CopyActivity1 byla úspěšně dokončena a Dataset2 je k dispozici. Tato datová sada se nepoužívá při kopírování dat tooDataset4. Pouze funguje jako plánování závislost pro CopyActivity2.   
* Dataset3 je k dispozici. Tato datová sada představuje hello data, která je zkopírovaný toohello cíl. 
