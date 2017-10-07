---
title: "Kanály aaaCreate nebo plánu, řetěz aktivity v datové továrně | Microsoft Docs"
description: "Další informace toocreate datový kanál v Azure Data Factory toomove a transformovat data. Vytvoření dat řízené informace připravené toouse tooproduce pracovního postupu."
keywords: "datový kanál, pracovní postup řízených daty"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 13b137c7-1033-406f-aea7-b66f25b313c0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: shlo
ms.openlocfilehash: 4a0fc20f98ce6453c16955e97fddb891926c173a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>Kanály a aktivity v Azure Data Factory
Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a použít tooconstruct začátku do konce řízené daty pracovních pro přesun dat a zpracování dat scénáře.  

> [!NOTE]
> Tento článek předpokládá, že jste prošli [Úvod tooAzure Data Factory](data-factory-introduction.md). Pokud nemáte hands-na-zkušenosti s vytvářením objektů pro vytváření dat, projít [kurzu transformaci dat](data-factory-build-your-first-pipeline.md) nebo [kurzu přesun dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) by vám pomohou pochopit, v tomto článku lépe.  

## <a name="overview"></a>Přehled
Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál je logické seskupení aktivit, které dohromady provádějí určitou úlohu. Hello aktivity v kanálu definují akce tooperform na vaše data. Můžete například použít kopie aktivity toocopy data z tooan systému SQL Server místní úložiště objektů Blob Azure. Poté použijte aktivitu Hive, která se spouští skript Hive na Azure HDInsight clusteru tooprocess nebo transformace dat z hello objektu blob úložiště tooproduce výstupní data. Nakonec použijte druhé kopie aktivity toocopy hello výstupní data tooan Azure SQL Data Warehouse nad kterou business intelligence (BI) jsou integrované řešení pro sestavy. 

Každá aktivita může mít nula nebo více vstupních [datových sad](data-factory-create-datasets.md) a může generovat jednu nebo více výstupních [datových sad](data-factory-create-datasets.md). Hello následující diagram znázorňuje hello vztah mezi kanálu, aktivity a datové sady v objektu pro vytváření dat: 

![Vztah mezi kanálu, aktivity a datové sady](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

Kanál vám umožní aktivity toomanage jako sada místo každé z nich jednotlivě. Můžete například nasadit, naplánovat, pozastavit a obnovit kanál, místo plánování práce s aktivitami v kanálu hello nezávisle.

Data Factory podporuje dva typy aktivit: aktivity přesunu dat a transformace dat. Každá aktivita může mít vstup nula nebo více [datové sady](data-factory-create-datasets.md) a vytvoří výstupní datové sady.

Vstupní datové sady reprezentuje hello vstup pro aktivitu v kanálu hello a výstupní datové reprezentuje hello výstup aktivity hello. Datové sady identifikují data v rámci různých úložišť dat, jako jsou tabulky, soubory, složky a dokumenty. Po vytvoření datové sady můžete tuto datovou sadu používat v aktivitách v rámci kanálu. Datová sada například může být vstupní/výstupní datovou sadou aktivity kopírování nebo aktivity HDInsightHive. Další informace o datových sadách najdete v článku [Datové sady v Azure Data Factory](data-factory-create-datasets.md).

### <a name="data-movement-activities"></a>Aktivity přesunu dat
Aktivita kopírování v datové továrně zkopíruje data z úložiště zdroje dat úložiště tooa podřízený data. Objekt pro vytváření dat podporuje hello následující datová úložiště. Data z jakéhokoli zdroje může být napsán tooany jímky. Klikněte na tlačítko data store toolearn jak toocopy tooand data z tohoto úložiště.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Úložiště dat, s * může být místní nebo v Azure IaaS a vyžadují tooinstall [Brána pro správu dat](data-factory-data-management-gateway.md) na počítači v místní nebo Azure IaaS.

Podrobnosti najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Aktivity transformace dat
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Podrobnosti najdete v článku [Aktivity transformace dat](data-factory-data-transformation-activities.md).

### <a name="custom-net-activities"></a>Vlastní aktivity .NET 
Pokud potřebujete toomove dat do/z dat nebude podporovat, nebo transformace dat pomocí vlastní logiky, vytvořte úložiště, které hello aktivity kopírování **vlastní aktivity .NET**. Podrobnosti o vytvoření a používání vlastní aktivity najdete v tématu [Použití vlastních aktivit v kanálu služby Azure Data Factory](data-factory-use-custom-activities.md).

## <a name="schedule-pipelines"></a>Plán kanálů
Kanál je aktivní jenom mezi jeho **spustit** čas a **end** čas. Nebude provedena před časem zahájení hello nebo po hello koncový čas. Když je pozastavená hello kanálu, se nebudou provedeny bez ohledu na jeho počáteční a koncový čas. Pro toorun kanálu by nemělo být pozastavena. V tématu [plánování a provádění](data-factory-scheduling-and-execution.md) toounderstand plánování a provádění fungování v Azure Data Factory.

## <a name="pipeline-json"></a>Zápis JSON kanálu
Teď se blíže podíváme na to, jak se kanál definuje ve formátu JSON. Obecná struktura Hello pro kanál vypadá takto:

```json
{
    "name": "PipelineName",
    "properties": 
    {
        "description" : "pipeline description",
        "activities":
        [

        ],
        "start": "<start date-time>",
        "end": "<end date-time>",
        "isPaused": true/false,
        "pipelineMode": "scheduled/onetime",
        "expirationTime": "15.00:00:00",
        "datasets": 
        [
        ]
    }
}
```

| Značka | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název kanálu hello. Zadejte název, který představuje hello akci, která hello kanálu provede. <br/><ul><li>Maximální počet znaků: 260.</li><li>Musí začínat písmenem, číslicí nebo podtržítkem (_).</li><li>Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Ano |
| description | Zadejte text hello popisující, co hello kanálu se používá pro. |Ano |
| activities | Hello **aktivity** části může mít jeden nebo více aktivity definované v něm. V části hello další podrobnosti o hello aktivity JSON elementu. | Ano |  
| start | Počáteční datum a čas pro kanál hello. Musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: `2016-10-14T16:32:41Z`. <br/><br/>Je možné toospecify místního času, například Odhadovaný čas. Tady je příklad: `2016-02-27T06:00:00-05:00`", což je odhadované AM 6<br/><br/>Hello počáteční a koncové vlastnosti společně zadejte aktivní období pro kanál hello. Výstup řezy jenom vytváří se v tomto aktivní období. |Ne<br/><br/>Pokud zadáte hodnotu pro vlastnost end hello, musíte zadat hodnotu pro vlastnost začátku hello.<br/><br/>Hello počáteční a koncový čas může být prázdný toocreate kanálu. Je potřeba zadat obě hodnoty tooset na aktivní období kanálu toorun hello. Pokud nezadáte počáteční a koncový čas při vytváření kanálu, můžete je nastavit pomocí rutiny Set-AzureRmDataFactoryPipelineActivePeriod hello později. |
| End | Koncové datum a čas pro kanál hello. Pokud zadaný, musí být ve formátu ISO. Příklad: `2016-10-14T17:32:41Z` <br/><br/>Je možné toospecify místního času, například Odhadovaný čas. Tady je příklad: `2016-02-27T06:00:00-05:00`, což je odhadované AM 6<br/><br/>9999-09-09 toorun hello kanálu bez omezení, zadejte jako hello hodnotu pro vlastnost end hello. <br/><br/> Kanál je aktivní jenom mezi její počáteční čas a koncový čas. Nebude provedena před časem zahájení hello nebo po hello koncový čas. Když je pozastavená hello kanálu, se nebudou provedeny bez ohledu na jeho počáteční a koncový čas. Pro toorun kanálu by nemělo být pozastavena. V tématu [plánování a provádění](data-factory-scheduling-and-execution.md) toounderstand plánování a provádění fungování v Azure Data Factory. |Ne <br/><br/>Pokud zadáte hodnotu pro vlastnost začátku hello, musíte zadat hodnotu pro vlastnost end hello.<br/><br/>Naleznete v poznámkách k hello **spustit** vlastnost. |
| isPaused | Pokud sada tootrue, hello kanálu nelze spustit. Je v hello pozastaví stavu. Výchozí hodnota = false. Můžete použít tento tooenable vlastnosti nebo zakázat kanál. |Ne |
| pipelineMode | Metoda Hello plánování spuštění pro hello kanálu. Povolené hodnoty jsou: naplánované (výchozí), jednorázově.<br/><br/>"Pravidelnou" označuje, že kanál hello se spustí v zadaném časovém intervalu podle tooits aktivní období (počáteční a koncový čas). "Jednorázově" označuje, že kanál hello spustí jenom jednou. Po vytvoření jednorázově kanály nelze aktuálně upravit nebo aktualizovat. V tématu [Onetime kanálu](#onetime-pipeline) podrobnosti o jednorázově nastavení. |Ne |
| ExpirationTime | Doba, po vytvoření pro které hello [jednorázového kanálu](#onetime-pipeline) je platná a by měla zůstat zřízené. Pokud nemá žádné aktivní, se nezdařilo, nebo až spuštění kanálu hello je automaticky odstraněna po dorazí hello čas vypršení platnosti. Výchozí hodnota Hello:`"expirationTime": "3.00:00:00"`|Ne |
| Datové sady |Seznam toobe datové sady používané aktivity definované v kanálu hello. Tuto vlastnost lze použít toodefine datové sady, které jsou specifické toothis kanálu a není definován v rámci objektu pro vytváření dat hello. Datové sady definované v rámci tohoto kanálu lze použít pouze tento kanál a nesmí se sdílet. V tématu [obor datové sady](data-factory-create-datasets.md#scoped-datasets) podrobnosti. |Ne |

## <a name="activity-json"></a>Zápis JSON aktivity
Hello **aktivity** části může mít jeden nebo více aktivity definované v něm. Každá aktivita má hello nejvyšší úrovně strukturu:

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
    },
    "scheduler":
    {
    }
}
```

Následující tabulka popisuje vlastnosti v aktivitě hello definici JSON:

| Značka | Popis | Požaduje se |
| --- | --- | --- |
| jméno | Název aktivity hello. Zadejte název, který představuje hello akce, který provádí aktivity hello. <br/><ul><li>Maximální počet znaků: 260.</li><li>Musí začínat písmenem, číslicí nebo podtržítkem (_).</li><li>Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> |Ano |
| description | Text popisující, co hello aktivity nebo se používá pro |Ano |
| type | Typ aktivity hello. V tématu hello [aktivity přesunu dat](#data-movement-activities) a [aktivit transformace dat](#data-transformation-activities) oddíly pro různé typy aktivit. |Ano |
| Vstupy |Vstupní tabulky použité aktivitou hello<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Ano |
| Výstupy |Výstupní tabulky použité aktivitou hello.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Ano |
| linkedServiceName |Název hello propojené služby používané hello aktivity. <br/><br/>Aktivita může vyžadovat, že zadáváte hello propojené služby, která propojí toohello požadované výpočetním prostředí. |Ano pro aktivita HDInsight a Azure Machine Learning aktivita dávkového vyhodnocování <br/><br/>Ne ve všech ostatních případech |
| typeProperties |Vlastnosti v hello **rámci typeProperties** části závisí na typu aktivity hello. vlastnosti typu toosee pro aktivity, klikněte na aktivitu toohello odkazů v předchozí části hello. | Ne |
| policy |Zásady, které ovlivňují chování běhové hello hello aktivity. Pokud není zadaný, použijí se výchozí zásady. |Ne |
| Scheduler | Vlastnost "scheduler" je použité toodefine potřeby plánování aktivity hello. Jeho podvlastnosti jsou hello stejné jako ty, které v hello hello [vlastnost availability v datové sadě](data-factory-create-datasets.md#dataset-availability). |Ne |


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

## <a name="sample-copy-pipeline"></a>Ukázkový kanál kopírování
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
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
} 
```

Všimněte si hello následující body:

* V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**.
* Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**. Informace o definicích datových sad ve formátu JSON najdete v článku [Datové sady](data-factory-create-datasets.md). 
* V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello. V hello [aktivity přesunu dat](#data-movement-activities) klikněte na úložiště dat, že chcete jako zdroj nebo podřízený toolearn více informací o přesun dat z tohoto úložiště dat toouse hello. 

Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Ukázkový kanál transformace
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
        "start": "2016-04-01T00:00:00Z",
        "end": "2016-04-02T00:00:00Z",
        "isPaused": false
    }
}
```

Všimněte si hello následující body: 

* V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**HDInsightHive**.
* soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **AzureStorageLinkedService**) a v  **skript** složky v kontejneru hello **adfgetstarted**.
* Hello `defines` část se nastavení používané toospecify hello běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Hello **rámci typeProperties** části se liší pro každou aktivitu transformace. toolearn o vlastnosti typu podporované pro transformaci aktivitu, klikněte na tlačítko hello transformace aktivity v hello [aktivit transformace dat](#data-transformation-activities) tabulky. 

Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: vytvoření vaší první dat tooprocess kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md). 

## <a name="multiple-activities-in-a-pipeline"></a>Více aktivit v kanálu
Hello předchozí dva ukázková kanály obsahují pouze jednu aktivitu. Kanál může obsahovat víc než jednu aktivitu.  

Pokud máte více aktivit v kanálu a výstup aktivity není vstup jinou aktivitu, může paralelně spustit hello aktivity, pokud připravení řezy vstupní data aktivity hello. 

Dvě aktivity můžete zřetězené tak, že hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Druhá aktivita Hello spustí jenom v případě, že hello nejdřív jednu úspěšně dokončena.

![Řetězení aktivity v hello stejné kanálu](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

V této ukázce hello kanálu má dvě aktivity: aktivity "activity1" a "activity2". Hello aktivity "activity1" trvá Dataset1 jako vstup a výstup vytváří Dataset2. Hello aktivity trvá Dataset2 jako vstup a výstup vytváří Dataset3. Od hello výstup aktivity "activity1" (Dataset2) je vstupem hello "activity2" hello "activity2" spustí až po úspěšném dokončení aktivity hello a vytváří hello Dataset2 řez. Pokud z nějakého důvodu selže hello aktivity "activity1" a nevytváří hello Dataset2 řez, hello Activity 2 pro tento řez nejde spustit (například: 9 AM too10 m). 

Můžete také zřetězené aktivity, které jsou v jiné kanály.

![Řetězení aktivity ve dvou kanálů](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

V této ukázce má Pipeline1 jenom jedna aktivita, která přebírá Dataset1 jako vstup a vytváří Dataset2 jako výstup. Hello Pipeline2 má také jenom jedna aktivita, která přebírá Dataset2 jako vstup a Dataset3 jako výstup. 

Další informace najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="create-and-monitor-pipelines"></a>Vytvoření a monitorování kanálů
Kanály můžete vytvořit pomocí jedné z těchto nástrojů nebo sady SDK. 

- Průvodce kopírováním. 
- portál Azure
- Visual Studio
- Azure PowerShell
- Šablona Azure Resource Manageru
- REST API
- .NET API

V tématu hello následující kurzy pro podrobné pokyny pro vytváření kanálů pomocí jedné z těchto nástrojů nebo sady SDK.
 
- [Vytvoření kanálu s aktivitou transformace dat](data-factory-build-your-first-pipeline.md)
- [Vytvoření kanálu s aktivitou přesun dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Jakmile kanál vytvořit nasazení, můžete spravovat a monitorovat hello kanály pomocí oken webu Azure portal nebo monitorování a správě aplikací. V tématu hello následující témata podrobné pokyny. 

- [Monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md).
- [Monitorování a Správa kanálů pomocí monitorování a Správa aplikací](data-factory-monitor-manage-app.md)


## <a name="onetime-pipeline"></a>Jednorázově kanálu
Můžete vytvořit a naplánovat kanálu toorun pravidelně (například: hodinové nebo denní) v rámci hello spuštění a ukončení zadáte v definici hello kanálu. V tématu [plánování aktivit](#scheduling-and-execution) podrobnosti. Můžete také vytvořit kanál, který se spustí jenom jednou. toodo tedy nastavíte hello **pipelineMode** vlastnost hello kanálu definice příliš**jednorázově** jak je znázorněno v následující ukázka JSON hello. Hello výchozí hodnota pro tuto vlastnost je **naplánované**.

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
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ]
                "name": "CopyActivity-0"
            }
        ]
        "pipelineMode": "OneTime"
    }
}
```

Vezměte na vědomí následující hello:

* **Spustit** a **end** nejsou zadány časy pro kanál hello.
* **Dostupnost** vstup a výstup je zadán datové sady (**frekvence** a **interval**), i když Data Factory hello hodnoty nepoužívá.  
* Zobrazení diagramu nezobrazuje jednorázové kanály. Toto chování je záměrné.
* Nelze aktualizovat, jednorázové kanály. Můžete klonovat jednorázového kanálu, přejmenujte ji, aktualizovat vlastnosti a nasadit toocreate jiný.


## <a name="next-steps"></a>Další kroky
- Další informace o datových sadách najdete v tématu [vytvoření datových sad](data-factory-create-datasets.md) článku. 
- Další informace o tom, jak jsou naplánované kanálů a jsou prováděny najdete v tématu [plánování a provádění v Azure Data Factory](data-factory-scheduling-and-execution.md) článku. 
  

