---
title: "podrobně aaaDeep předpovědi vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti hello Cortana Intelligence toogain v reálném čase a prediktivní Statistika na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: ba1448a5081762292561f904d9ec54617c9a5330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-hello-solution"></a>Vehicle telemetrie analytics řešení playbook: podrobné informace do řešení hello
To **nabídky** odkazy toohello části této playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Tato část podrobněji zanalyzuje data do v jednotlivých fázích hello znázorněný v hello architektury řešení s pokyny a ukazatele pro přizpůsobení. 

## <a name="data-sources"></a>Zdroje dat
řešení Hello používá dvou různých zdrojů dat.:

* **simulated vehicle signály a diagnostiky datovou sadu** a 
* **vehicle katalogu**

Vehicle telematika jsme je součástí tohoto řešení. Vysílá diagnostické informace a signály odpovídající toohello stav hello vehicle a toohello řídí vzor k danému bodu v čase. Klikněte na tlačítko [Vehicle telematika simulátoru](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle telematika simulátoru Visual Studio řešení** přizpůsobení podle svých požadavků. Hello vehicle katalogu obsahuje odkaz na datovou sadu s VIN toomodel mapování.

![Vehicle telematika simulátoru](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

*Obrázek 1 – Vehicle telematika simulátoru*

Toto je datovou sadu formátu JSON, který obsahuje následující schéma hello.

| Sloupec | Popis | Hodnoty |
| --- | --- | --- |
| VIN |Náhodně generované identifikační číslo |To se získávají z hlavní seznam 10 000 náhodně generované vehicle identifikačními čísly. |
| Mimo teploty |Hello mimo teploty, které řídí hello vehicle |Náhodně generované číslo od 0 – 100 |
| Modul teploty |modul teplota Hello hello vehicle |Náhodně generované číslo od 0-500 |
| Rychlost |rychlost Hello modul, na které hello vehicle řídí |Náhodně generované číslo od 0 – 100 |
| Paliva |úroveň paliva Hello hello vehicle |Náhodně generované číslo od 0 až 100 (určuje procento úrovně paliva) |
| EngineOil |Hello modul těžba ropy úroveň hello vehicle |Náhodně generované číslo od 0 až 100 (určuje procento úrovně těžba ropy modul) |
| Můžete zadat přetížení |můžete zadat tlak Hello hello vehicle |Náhodně generované číslo od 0 – 50 (určuje procento úrovně zatížení můžete zadat) |
| Počítadlo kilometrů |čtení počítadlo kilometrů Hello vozidel hello |Náhodně generované číslo od 0 200000 |
| Accelerator_pedal_position |Hello akcelerátoru Pedálové pozici hello vehicle |Náhodně generované číslo od 0 až 100 (určuje procento úrovně akcelerátoru) |
| Parking_brake_status |Určuje, zda text hello vehicle zaparkováno nebo ne |True nebo False |
| Headlamp_status |Určuje, kde je hello světlometu na nebo ne |True nebo False |
| Brake_pedal_status |Určuje, zda text hello brzdového pedálu stisknutí nebo ne |True nebo False |
| Transmission_gear_position |Hello přenosu ozubené kolečko pozici hello vehicle |Stavy: první, druhá, třetí a čtvrté, páté, šestého, sedmá, osmého |
| Ignition_status |Určuje, zda je spuštěná nebo zastavená hello vehicle |True nebo False |
| Windshield_wiper_status |Určuje, zda stírání čelního skla hello zapnutý, nebo ne |True nebo False |
| ABS |Určuje, zda je nebo není zařazen ABS |True nebo False |
| časové razítko |časové razítko Hello při vytvoření hello datového bodu |Datum |
| Město |umístění Hello hello vehicle |4 města v tomto řešení: Bellevue, Redmond, Sammamish, Praha |

Hello vehicle modelu referenční datová sada obsahuje VIN toohello modelu mapování. 

| VIN | Model |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Limuzíny |
| 8J0U8XCPRGW4Z3NQE |Hybridní |
| WORG68Z2PLTNZDBI7 |Rodiny Sedan |
| JTHMYHQTEPP4WBMRN |Limuzíny |
| W9FTHG27LZN1YWO0Y |Hybridní |
| MHTP9N792PHK08WJM |Rodiny Sedan |
| EI4QXI2AXVQQING4I |Limuzíny |
| 5KKR2VB4WHQH97PF8 |Hybridní |
| W9NSZ423XZHAONYXB |Rodiny Sedan |
| 26WJSGHX4MA5ROHNL |Převoditelné |
| GHLUB6ONKMOSI7E77 |Kombi |
| 9C2RHVRVLMEJDBXLP |Compact Car |
| BRNHVMZOUJ6EOCP32 |Malé SUV |
| VCYVW0WUZNBTM594J |Auto sportu |
| HNVCE6YFZSA5M82NY |Střední SUV |
| 4R30FOR7NUOBL05GJ |Kombi |
| WYNIIY42VKV6OQS1J |Velké SUV |
| 8Y5QKG27QET1RBK7I |Velké SUV |
| DF6OX2WSRA6511BVG |Kupé |
| Z2EOZWZBXAEW3E60T |Limuzíny |
| M4TV6IEALD5QDS3IR |Hybridní |
| VHRA1Y2TGTA84F00H |Rodiny Sedan |
| R0JAUHT1L1R3BIKI0 |Limuzíny |
| 9230C202Z60XX84AU |Hybridní |
| T8DNDN5UDCWL7M72H |Rodiny Sedan |
| 4WPYRUZII5YV7YA42 |Limuzíny |
| D1ZVY26UV2BFGHZNO |Hybridní |
| XUF99EW9OIQOMV7Q7 |Rodiny Sedan |
| 8OMCL3LGI7XNCC21U |Převoditelné |
| ……. | |

### <a name="references"></a>Odkazy
[Řešení vehicle telematika simulátoru sady Visual Studio](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Centra událostí Azure](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a>Přijímání
Kombinace Azure Event Hubs, Stream Analytics a objektu pro vytváření dat jsou využity tooingest hello vehicle signály, hello diagnostických událostí a v reálném čase a analýza služby batch. Všechny tyto součásti vytvoříte a nakonfigurujete jako součást nasazení řešení hello. 

### <a name="real-time-analysis"></a>Analýzy v reálném čase
Hello událostí generovaných hello Vehicle telematika simulátoru jsou publikované pomocí centra událostí toohello hello SDK centra událostí. Úloha Stream Analytics Hello ingestuje tyto události z hello centra událostí a procesy hello data v reálném čase tooanalyze hello vehicle stavu. 

![Řídicí panel centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

*Obrázek 4 – řídicí panel centra událostí*

![Stream Analytics úlohy zpracování dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Obrázek 5 – úloha Stream Analytics zpracování dat*

Úloha Stream Analytics Hello;

* ingestuje data z hello centra událostí 
* provede připojení k s hello referenční data toomap hello vehicle VIN toohello odpovídající model 
* dál je do Azure blob storage analytics bohaté batch. 

Následující dotaz služby Stream Analytics Hello je použité toopersist hello data do úložiště objektů blob Azure. 

![Dotaz úlohy Stream Analytics přijímání dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Obrázek 6 - Stream Analytics úlohy dotazu pro přijímání dat*

### <a name="batch-analysis"></a>Dávková analýza
Také jsme generují další svazek simulované vehicle signály a diagnostiky datové sady pro širší batch analýzu. Toto je požadovaná tooensure dobrý reprezentativní datový svazek pro dávkové zpracování. Pro tento účel používáme kanál s názvem "PrepareSampleDataPipeline" v toogenerate pracovní postup Azure Data Factory hello za jeden rok simulované vehicle signály a diagnostiky datové sady. Klikněte na tlačítko [vlastní aktivity služby Data Factory](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello aktivity DotNet vlastní objekt pro vytváření dat řešení sady Visual Studio pro přizpůsobení podle svých požadavků. 

![Příprava ukázková data pro dávkové zpracování pracovního postupu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Obrázek 7: Příprava ukázková data pro zpracování pracovní postup služby batch*

Hello kanálu se skládá z vlastní .net ADF aktivity, zobrazit tady:

![PrepareSampleDataPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Obrázek 8 - PrepareSampleDataPipeline*

Jakmile hello kanálu provede úspěšně a datovou sadu "RawCarEventsTable" je označen jako "Připravený", jeden rok vhodné simulované vehicle signálů a diagnostických dat vytváří. Zobrazí hello následující složku a soubor vytvořený v účtu úložiště v kontejneru "connectedcar" hello:

![Výstup PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Obrázek 9 - PrepareSampleDataPipeline výstup*

### <a name="references"></a>Odkazy
[Azure SDK centra událostí pro přijímání datového proudu](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Možnosti přesun dat Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
[aktivita služby Azure Data Factory DotNet.](../data-factory/data-factory-use-custom-activities.md)

[Řešení sady visual studio Azure Data Factory DotNet aktivity pro přípravu ukázková data](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-hello-dataset"></a>Oddíl hello datové sady
signály nezpracovaná částečně strukturovaných vehicle Hello a diagnostiky datové sady jsou rozděleny do oddílů v kroku přípravy hello dat do formátu měsíc roku. Toto rozdělení do oddílů zvýší úroveň efektivnější dotazování a škálovatelné dlouhodobé úložiště povolením odolnost převzetí z jednoho objektu blob účet toohello vedle jako první účet hello zaplní. 

>[!NOTE] 
>Tento krok v řešení hello je použít jenom toobatch zpracování.

Vstupní a výstupní data správy dat:

* Hello **výstupní data** (s názvem bez přípony *PartitionedCarEventsTable*) se ukládají jako toobe na dlouhou dobu jako formulář základní / "rawest" hello dat v hello zákazníka "Data Lake". 
* Hello **vstupní data** toothis kanálu by obvykle odstraněn jako hello výstupních dat má úplné věrnosti toohello vstup – stačí uložené (oddíly) lépe pro pozdější použití.

![Pracovní postup události car oddílu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

*Obrázek 10: pracovní postup události Car oddílu*

nezpracovaná data Hello je rozdělen na oddíly pomocí aktivitu Hive HDInsight v "PartitionCarEventsPipeline". Ukázková data Hello vygenerované v roce v kroku 1 je rozdělena na oddíly pomocí měsíc roku. Hello oddíly jsou signály použité toogenerate vehicle a diagnostických dat pro každý měsíc (celkový počet 12 oddíly) v roce. 

![PartitionCarEventsPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

*Obrázek 11 – PartitionCarEventsPipeline*

***Skript PartitionConnectedCarEvents Hive***

Hello následující skript Hive, s názvem "partitioncarevents.hql", slouží k vytváření oddílů a je umístěn ve složce "\demo\src\connectedcar\scripts" hello hello stáhnout zip. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.

![Oddílů výstup](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

*Obrázek 12 - oddílů výstup*

Hello dat je nyní optimalizovaná, správu a připraveno pro další zpracování toogain bohaté batch statistiky. 

## <a name="data-analysis"></a>Analýza dat
V této části, uvidíte, jak toocombine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory a Azure HDInsight pro bohaté pokročilé analýzy vehicle stavu a řídí zvyklosti. Existují tři pododdíly tady:

1. **Strojového učení**: Tato část obsahuje informace o hello experimentu detekce anomálií, který jsme použili v vozidel toopredict toto řešení nutnosti údržby údržby a nutnosti navrácení z důvodu problémů toosafety vozidel.
2. **Analýzy v reálném čase**: Tato část obsahuje informace týkající se analýzy hello v reálném čase pomocí hello Stream Analytics Query Language a zprovozňování hello strojové učení experiment v reálném čase pomocí vlastní aplikaci.
3. **Batch analysis**: Tato část obsahuje informace týkající se hello transformaci a zpracování dat hello batch pomocí Azure HDInsight nebo Azure Machine Learning operationalized službou Azure Data Factory.

### <a name="machine-learning"></a>Machine Learning
Zde Naším cílem je toopredict hello vozidel, které vyžadují údržby nebo odvolání na základě statistik určité stavu. Provedeme následující předpoklady hello

* Pokud platí jedna z následujících tří podmínek hello, vyžadují hello vozidel **obsluhy údržby**:
  
  * Můžete zadat naléhavost je nízká
  * Modul těžba ropy úroveň je nízká
  * Modul teploty je vysoká.
* Pokud platí jedna z následujících podmínek hello, může mít hello vozidel **problém zabezpečení** a vyžadovat **odvolání**:
  
  * Modul teploty je vysoká, ale mimo teploty je nízká
  * Modul teploty je nízký, ale mimo teploty je vysoká.

Na základě hello předchozí požadavků, jsme vytvořili dva samostatné modely toodetect anomálií, jeden pro zjišťování vehicle údržby a jeden pro zjišťování pro vyvolání vehicle. V obou těchto modelech hello předdefinované hlavní součásti Analysis (PCA) algoritmus se používá pro detekce anomálií. 

**Model pro odhalování údržby**

Pokud jeden z tři indikátory - zatížení můžete zadat, těžba ropy modul nebo modul teploty - splňuje jeho odpovídající stav, hello údržby detekce modelu sestavy anomálií. V důsledku toho pouze potřebujeme tooconsider těchto tří proměnných v sestavení hello modelu. V našem experimentu v Azure Machine Learning, nejprve používáme **výběr sloupců v datové sadě** modulu tooextract těchto tří proměnných. Další používáme hello anomálií založený na PCA detekce modulu toobuild hello anomálií detekce modelu. 

Hlavní součásti Analysis (PCA) je zavedených technika v machine learning, které můžou být použité toofeature výběr, klasifikace a anomálií detekce. PCA převede sadu případ obsahující pravděpodobně korelační proměnné, do sady hodnot nazývané jako hlavní komponenty. Hello klíče představu o tom, na základě PCA modelování je tooproject dat na nižší dimenzí místa, aby funkce a anomálií, lze snadno identifikovat.

Pro každý nový vstup příliš hello model pro odhalování, detekce anomálií hello nejprve vypočítá jeho projekce na hello eigenvectors a pak výpočtů hello normalized obnovu chyby. Tato chyba normalizovaný je hello anomálií skóre. Hello vyšší hello chyba hello více neobvyklé hello instance je. 

V hello údržby zjišťování problému každý záznam lze považovat za bod v prostorovém 3 prostoru definovaný zatížení můžete zadat, modul těžba ropy a teploty modul souřadnice. toocapture tyto anomálií jsme projektu hello původní data v prostoru 3 dimenzí hello na 2 dimenzí prostor pomocí PCA. Proto jsme nastavte parametr hello počet toouse součásti v PCA toobe 2. Tento parametr hraje důležitou roli při uplatňování detekce anomálií založený na PCA. Po plánování dat pomocí PCA abychom mohli snadno identifikovat tyto anomálií.

**Model detekce anomálií odvolání** v modelu detekce anomálií hello odvolání používáme hello výběr sloupců v datové sadě a anomálií založený na PCA moduly rozpoznávání podobným způsobem. Konkrétně jsme nejprve extrahovat tří proměnných - teplota motoru, mimo teploty a rychlosti – pomocí hello **výběr sloupců v datové sadě** modulu. Můžeme také zahrnovat hello rychlost proměnná vzhledem k tomu, že teplota motoru hello je obvykle korelační toohello rychlost. Další používáme anomálií založený na PCA detekce modulu tooproject hello data z hello 3 dimenzí prostoru na 2 dimenzí. jsou splněna kritéria pro vyvolání Hello a proto hello vehicle při vysoce negativně korelační modul teploty a mimo teploty vyžaduje odvolání. Pomocí algoritmu detekce anomálií založený na PCA, jsme po provedení PCA zachytit hello anomálií. 

Cvičení buď modelu, potřebujeme toouse normální data, která nevyžaduje údržby nebo odvolání jako model detekce anomálií založený na PCA tootrain hello aplikace hello vstupní data. V hello vyhodnocování experiment použijeme toodetect modelu detekce anomálií hello Trénink zda hello vehicle vyžaduje údržby nebo odvolání. 

### <a name="real-time-analysis"></a>Analýzy v reálném čase
Následující dotaz SQL Stream Analytics Hello slouží tooget hello průměr všech hello parametry důležité vehicle jako rychlosti, úroveň paliva, modul teploty, počítadlo kilometrů čtení, můžete zadat naléhavost, modul těžba ropy úroveň a dalších. Hello průměry jsou použité toodetect anomálií, vydávání výstrah a určit hello celkového stavu podmínky vozidel provozovat v určité oblasti a pak ho korelovat toodemographics. 

![Stream Analytics query pro zpracování v reálném čase](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

*Obrázek 13: dotaz služby Stream Analytics ke zpracování v reálném čase*

Všechny průměry hello jsou vypočítávány přes TumblingWindow 3 sekundu. TubmlingWindow se používá v tomto případě vzhledem k tomu, že je nutné, aby se a souvislý časové intervaly. 

Klikněte na tlačítko Další informace o všechny možnosti "Oddílová" hello v Azure Stream Analytics toolearn [Oddílová (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**V reálném čase předpovědi**

Aplikace je součástí model hello řešení toooperationalize hello machine learning v reálném čase. Tuto aplikaci s názvem "RealTimeDashboardApp" je vytvořen a nakonfigurován jako součást nasazení řešení hello. aplikace Hello provádí hello následující:

1. Naslouchá instance tooan centra událostí, kde Stream Analytics je publikování hello událostí ve tvaru nepřetržitě. ![Stream Analytics dotazu pro publikování dat hello](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *obrázek 14: Stream Analytics dotazu pro publikování dat tooan hello výstup instance centra událostí* 
2. Pro každou událost, která obdrží tuto aplikaci: 
   
   * Data hello procesů používá koncový bod Machine Learning požadavků a odpovědí vyhodnocování (záznamy RR). Hello záznamy o prostředku koncového bodu je automaticky publikován jako součást nasazení hello.
   * výstup RRS Hello je datová sada publikované tooa Power BI pomocí nabízených hello rozhraní API.

Tento vzor je také použít tooscenarios, ve kterém chcete toointegrate obchodní (LoB) aplikace s tokem hello analýzu v reálném čase, pro scénáře, jako je výstrahy, oznámení a zasílání zpráv.

Klikněte na tlačítko [RealtimeDashboardApp stažení](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello řešení sady Visual Studio RealtimeDashboardApp pro úpravy. 

**tooexecute hello aplikace řídicího panelu v reálném čase**
1. Extrahování a uložit místně ![RealtimeDashboardApp složky](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *obrázek 16 – RealtimeDashboardApp složky*  
2. Spuštění aplikace hello RealtimeDashboardApp.exe
3. Zadejte platné přihlašovací údaje Power BI, přihlášení a klikněte na tlačítko Přijmout ![Řídicí panel v reálném čase aplikace přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Aplikace v reálném čase řídicího panelu dokončit přihlášení tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Obrázek 17 – RealtimeDashboardApp: Přihlášení tooPower BI*

>[!NOTE] 
>Pokud chcete datovou sadu Power BI hello tooflush, spusťte hello RealtimeDashboardApp s parametrem "flushdata" hello: 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Dávková analýza
Hello cílem je, že tooshow jak motory Contoso využívá hello výpočtů Azure možnosti tooharness velkých objemů dat toogain detailní přehled na řízení vzorek, chování využití a stavu vehicle. Díky tomu je možné:

* Zlepšovat prostředí zákazníků hello a nastavit jej jako levnější tím, že poskytuje přehled na řídí zvyklosti a efektivní řízení chování paliva
* Další informace proaktivně o zákazníků a jejich řízení patters toogovern obchodních rozhodnutí a poskytují hello nejlépe v třída produkty a služby

V tomto řešení jsme cílení hello následující metriky:

1. **Agresivní řídí chování**: identifikuje hello trend hello modely, umístění, podporovat jeho podmínky a čas hello roku toogain Statistika agresivní řízení vzory. Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí přizpůsobené nové funkce a pojišťovnictví na základě využití.
2. **Paliva efektivní řízení chování**: identifikuje hello trend hello modely, umístění, podporovat jeho podmínky a čas hello roku toogain Statistika paliva efektivní řízení vzory. Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí nové funkce a proaktivní reporting toohello ovladače pro náklady platné a prostředí zvyklosti popisný řízení. 
3. **Odvolat modely**: identifikuje modely nutnosti navrácení podle zprovozňování hello strojové učení detekce anomálií experimentu

Podívejme se na podrobnosti hello jednotlivých tyto metriky

**Agresivní řízení vzor**

Hello vehicle signály rozdělena na oddíly a diagnostických dat jsou zpracovávány v hello kanál s názvem "AggresiveDrivingPatternPipeline" pomocí Hive toodetermine hello modely, umístění, vehicle, řídí podmínky a dalších parametrů, které jádro vykazuje agresivní řídí vzor.

![Agresivní řízení pracovního postupu vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*obrázek 18 – agresivní řízení pracovního postupu vzor*


***Agresivní řízení dotaz Hive vzor***

Hello skriptu Hive s názvem "aggresivedriving.hql" použít pro analýzu agresivní řízení vzor podmínka je umístěný ve složce "\demo\src\connectedcar\scripts" hello stáhnout zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


Používá kombinaci hello vehicle na pozici přenosu ozubené kolečko, brzdy Pedálové stav a rychlost toodetect reckless/agresivní řídí chování v závislosti na brzdění vzor vysokou rychlostí. 

Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.

![Výstup AggressiveDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Obrázek 19 – AggressiveDrivingPatternPipeline výstup*

**Efektivní řízení vzor paliva**

Hello vehicle signály rozdělena na oddíly a diagnostických dat jsou zpracovány v hello kanál s názvem "FuelEfficientDrivingPatternPipeline". Hive je použité toodetermine hello modely, umístění, vehicle, jízdních podmínek a další vlastnosti, které vykazují paliva efektivní řízení vzorem.

![Zvýšení řízení vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Obrázek 20 – zvýšení řízení pracovního postupu vzor*

***Paliva efektivní řízení vzor dotaz Hive***

Hello skriptu Hive s názvem "fuelefficientdriving.hql" použít pro analýzu agresivní řízení vzor podmínka je umístěný ve složce "\demo\src\connectedcar\scripts" hello stáhnout zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Používá kombinaci hello přenosu ozubené kolečko pozice vehicle na, brzdy Pedálové stav, rychlost a akcelerátoru Pedálové pozice toodetect paliva efektivní řízení chování podle akcelerace, brzdění, a rychlost vzory. 

Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.

![Výstup FuelEfficientDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Obrázek 21 – FuelEfficientDrivingPatternPipeline výstup*

**Odvolat předpovědi**

Hello experimentu strojového učení je zřízený a publikovat jako webovou službu, jako součást nasazení řešení hello. v tomto pracovním postupu, zaregistrován jako služba data factory propojené a operationalized pomocí data factory aktivita dávkového vyhodnocování se využívají záznamy Hello dávkového vyhodnocování koncového bodu.

![Koncový bod Machine Learning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

*Obrázek 22 – strojového učení koncový bod registrován jako propojené služby v datové továrně*

Hello registrovanou propojenou službu se používá v hello DetectAnomalyPipeline tooscore hello data pomocí modelu detekce anomálií hello. 

![Počítač Learning aktivita dávkového vyhodnocování v datové továrně](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

*Obrázek 23 – Azure Machine Learning dávkového vyhodnocování aktivity v objektu pro vytváření dat* 

Je několik kroků provést u tohoto kanálu pro přípravu dat tak, aby může být operationalized s hello dávkového vyhodnocování webové služby. 

![DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

*Obrázek 24 – DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení* 

***Dotaz Hive detekce anomálií***

Po dokončení hello vyhodnocování aktivita HDInsight je použité tooprocess a hello agregovaná data, která jsou klasifikovány jako anomálií modelem hello se skóre pravděpodobnosti 0,60 nebo vyšší.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Jakmile hello kanálu proveden úspěšně, zobrazí následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar" hello hello.

![Výstup DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Obrázek 25 – DetectAnomalyPipeline výstup*

## <a name="publish"></a>Publikování

### <a name="real-time-analysis"></a>Analýzy v reálném čase
Jeden z hello dotazy v úloze Stream Analytics hello publikuje hello události tooan výstup instance centra událostí. 

![Úlohy Stream Analytics publikuje tooan výstup instance centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Obrázek 26 – úloha Stream Analytics publikuje tooan výstup instance centra událostí*

![Stream Analytics query toopublish toohello výstup instance centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Obrázek 27 – Stream Analytics query toopublish toohello výstup instance centra událostí*

Tento datový proud událostí, které se spotřebovávají hello RealTimeDashboardApp součástí hello řešení. Tato aplikace využívá hello Machine Learning požadavků a odpovědí webové služby pro vyhodnocování v reálném čase a publikuje hello datovou sadu Power BI tooa Výsledná data pro používání. 

### <a name="batch-analysis"></a>Dávková analýza
výsledky Hello hello batch a v reálném čase zpracování jsou publikované toohello tabulky Azure SQL Database pro používání. Hello Azure SQL Server, databáze a tabulky hello se vytvoří automaticky v rámci hello instalační skript. 

![Výsledky zpracování dávky zkopírujte toodata Tržiště pracovního postupu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Obrázek 28 – dávkového zpracování výsledků zkopírování toodata Tržiště pracovního postupu*

![Úlohy Stream Analytics publikuje toodata Tržiště](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Obrázek 29 – úloha Stream Analytics publikuje toodata Tržiště*

![Datové Tržiště nastavení v úloze Stream Analytics](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Obrázek 30 – datové Tržiště nastavení v úloze Stream Analytics*

## <a name="consume"></a>Konzumace
Power BI poskytuje toto řešení bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy. 

Kliknutím sem získáte podrobné pokyny k nastavení hello Power BI sestavy a řídicí panel hello. řídicí panel konečné Hello vypadá takto:

![Řídicí panel Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

*Obrázek 31 - řídicí panel Power BI*

## <a name="summary"></a>Souhrn
Tento dokument obsahuje podrobné procházení Dobrý den řešení analýzy Vehicle Telemetrie. To umožňující prezentovat vzor architektura lambda v reálném čase a dávky analytics s předpovědi a akce. Tento vzor platí tooa širokou škálu případy použití, které vyžadují aktivní cesta (v reálném čase) a analýzy neaktivní trase (batch). 

