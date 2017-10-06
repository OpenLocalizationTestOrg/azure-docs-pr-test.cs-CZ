---
title: "aaaAzure Machine Learning anomálií detekce API | Microsoft Docs"
description: "Rozhraní API detekce anomálií je příklad vytvořené s nástroji Microsoft Azure Machine Learning, který zjistí anomálie v časových řad dat se číselné hodnoty, které jsou rozloženy rovnoměrně v čase."
services: machine-learning
documentationcenter: 
author: alokkirpal
manager: jhubbard
editor: cgronlun
ms.assetid: 52fafe1f-e93d-47df-a8ac-9a9a53b60824
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/05/2017
ms.author: alok;rotimpe
ms.openlocfilehash: ce153689b8ddb36b67a2ad3607d846ea83ebcf61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-anomaly-detection-api"></a>Strojového učení detekce anomálií rozhraní API
## <a name="overview"></a>Přehled
[Rozhraní API detekce anomálií](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) je příklad vytvořené s nástroji Azure Machine Learning, který zjistí anomálie v časových řad dat se číselné hodnoty, které jsou rozloženy rovnoměrně v čase.

Toto rozhraní API můžete zjistit hello následující typy neobvyklé vzory v časových řad dat:

* **Kladné a záporné trendy**: při monitorování využití paměti v oblasti výpočetních vzestupný trend může být například týkající se může být naznačuje výslednou nevracení paměti
* **Změny v rozsahu hodnot hello dynamické**: například při monitorování hello výjimky vyvolané Cloudová služba, všechny změny v hello dynamické rozsahu hodnot může znamenat nestabilitě hello stav hello služby, a
* **Špičky a klesne**: například při monitorování hello počet neúspěšných přihlášení ve službě nebo počet rezervace web elektronického obchodu, špičky nebo vyhrazené IP adresy by mohly být známkou neobvyklé chování.

Tyto machine learning detektory sledovat tyto změny v hodnotách průběhu čas a sestava probíhající změny v jejich hodnoty jako anomálií skóre. Nevyžadují ad hoc prahová hodnota ladění a jejich skóre lze použít toocontrol false kladné rychlost. detekce anomálií Hello rozhraní API je užitečné v několika scénářích jako monitorování služby pomocí funkce sledování klíčových ukazatelů výkonu v čase, využití monitorování prostřednictvím metriky, například počet vyhledávání, počet kliknutí na sledování výkonu prostřednictvím počítadla jako paměť, procesor, soubor načte, atd. v čase.

Nabídka detekce anomálií Hello se dodává s tooget užitečné nástroje, které jste spustili.

* Hello [webové aplikace](http://anomalydetection-aml.azurewebsites.net/) vám pomůže vyhodnotit a vizualizovat hello výsledky detekce anomálií rozhraní API na vaše data.

> [!NOTE]
> Zkuste **řešení IT anomálií Insights** používá technologii [toto rozhraní API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)
> 
> tooget této koncoví tooend řešení nasazeno tooyour předplatného Azure <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **Začněte zde >**</a>
> 
>

## <a name="api-deployment"></a>Rozhraní API nasazení
V pořadí toouse hello rozhraní API je nutné nasadit tooyour předplatné Azure, kde se bude hostovat jako webové služby Azure Machine Learning.  Můžete to provést z hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2).  To nasadí dvě AzureML webové služby (a jejich související prostředky) tooyour předplatného Azure – jeden pro zjišťování anomálií s sezónnosti detekce a jeden bez detekce sezónnosti.  Po dokončení nasazení hello nebudete moct toomanage vaše rozhraní API z hello [AzureML webové služby](https://services.azureml.net/webservices/) stránky.  Z této stránky můžete bude být schopný toofind umístění koncových bodů, klíče rozhraní API, a také ukázkový kód pro volání rozhraní API hello.  Podrobnější pokyny jsou k dispozici [zde](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice).

## <a name="scaling-hello-api"></a>Škálování hello rozhraní API
Ve výchozím nastavení bude mít vaše nasazení plán fakturace volné vývoje/testování, která zahrnuje 1000 transakce za měsíc a 2 výpočetních hodin za měsíc.  Můžete upgradovat plán tooanother podle vašich potřeb.  Podrobnosti o cenách hello z různých plánů jsou k dispozici [sem](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) v části "Produkční webové rozhraní API ceny".

## <a name="managing-aml-plans"></a>Správa AML plány 
Můžete spravovat cenového plánu [zde](https://services.azureml.net/plans/).  Název plánu Hello budou založeny na hello název skupiny prostředků, kterou jste zvolili při nasazení hello API plus řetězec, který je jedinečný tooyour předplatné.  Pokyny, jak tooupgrade plánu jsou k dispozici [sem](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice) části "Správa fakturace plány" hello.

## <a name="api-definition"></a>Definice rozhraní API.
Hello webová služba poskytuje rozhraní REST API přes protokol HTTPS, které mohou být využívány různými způsoby, včetně webových a mobilních aplikací, R, Python, Excel, atd.  Odeslat služby čas řady dat toothis prostřednictvím volání rozhraní REST API a běží kombinaci tři typy anomálií hello popsané dole.

## <a name="calling-hello-api"></a>Volání rozhraní API hello
V pořadí toocall hello rozhraní API budete potřebovat tooknow hello koncový bod umístění a klíč rozhraní API.  Obě tyto, společně s ukázkový kód pro volání rozhraní API hello, jsou k dispozici na hello [AzureML webové služby](https://services.azureml.net/webservices/) stránky.  Přejděte toohello potřeby rozhraní API a potom klikněte na toofind kartě "Spotřebě" hello je.  Všimněte si, že můžete volat rozhraní API hello jako rozhraní API Swaggeru (tj. s parametr URL hello `format=swagger`) nebo jako jiný – rozhraní API Swaggeru (tedy bez hello `format` parametr URL).  Hello ukázkový kód používá formát Swagger hello.  Níže je příklad požadavku a odpovědi ve formátu bez Swagger.  Tyto příklady jsou toohello sezónnosti koncový bod.  koncový bod není sezónnosti Hello je podobný.

### <a name="sample-request-body"></a>Ukázkový text žádosti
Hello požadavek obsahuje dva objekty: `Inputs` a `GlobalParameters`.  V níže uvedenou žádost příklad hello, některé parametry jsou odesílány explicitně zatímco jiné nejsou (přejděte dolů pro úplný seznam parametrů pro každý koncový bod).  Parametry, které nejsou explicitně odeslaných v požadavku hello použije výchozí hodnoty hello vypsáni níže.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Ukázková odezva
Všimněte si, že, v pořadí toosee hello `ColumnNames` pole, je nutné zahrnout `details=true` jako parametr adresy URL ve vaší žádosti.  Najdete v části tabulky hello pod pro hello význam za každé z těchto polí.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Rozhraní API skóre
Hello skóre rozhraní API se používá pro spouštění na data řady-sezónní čas detekce anomálií. Hello API spustí počet anomálií detektory hello data a vrátí jejich anomálií skóre. Hello obrázek níže znázorňuje příklad anomálií skóre API může zjistit, že hello. Toto časové řady má 2 odlišné úrovně změny a 3 špičky. Hello červené tečky nezobrazovat hello čas, na které hello je zjištěna změna úrovně, při hello černé tečky zobrazit hello zjistil špičky.
![Rozhraní API skóre][1]

### <a name="detectors"></a>Detektory
detekce anomálií Hello rozhraní API podporuje detektory ve 3 rozsáhlé kategorie. Podrobnosti o konkrétní vstupní parametry a výstupy pro každý detektor naleznete v následující tabulce hello.

| Detektor kategorie | Detektor | Popis | Vstupní parametry | Výstupy |
| --- | --- | --- | --- | --- |
| Detektory Špička |Detektor TSpike |Zjištění špičky a vyhrazené IP adresy podle hodnoty jsou od první a třetí Kvartily úplně hello |*tspikedetector.Sensitivity:* trvá celočíselná hodnota v rozsahu hello výchozí 1 – 10: 3; Vyšší hodnoty zachytí další extrémní hodnoty, díky čemuž méně citlivou |TSpike: binární hodnoty – 1, pokud je zjištěn Špička/dip, '0' jinak hodnota |
| Detektory Špička | Detektor ZSpike |Zjištění špičky a vyhrazené IP adresy založené na tom, jak úplně datapoints hello se od jejich střední hodnoty |*zspikedetector.Sensitivity:* trvat 1 – 10, výchozí hodnota celé číslo v rozsahu hello: 3; Vyšší hodnoty zachytí další extrémní hodnoty, což méně citlivou |ZSpike: binární hodnoty – 1, pokud je zjištěn Špička/dip, '0' jinak hodnota | |
| Detektor pomalé trendu |Detektor pomalé trendu |Zjištění pomalé pozitivní trend podle hello sadu velkých a malých písmen |*trenddetector.Sensitivity:* prahovou hodnotu na detektor skóre (výchozí: 3,25, 3,25 – 5 jedná se rozsah přiměřené tooselect z; hello vyšší hello méně velká a malá písmena) |tscore: plovoucí desetinné číslo představující skóre anomálií na trendu |
| Změna úrovně detektory | Detektor změnit úroveň obousměrného |Zjištění obou směrem nahoru a dolů úrovně změnu podle hello nastavit citlivost |*bileveldetector.Sensitivity:* prahovou hodnotu na detektor skóre (výchozí: 3,25, 3,25 – 5 jedná se rozsah přiměřené tooselect z; hello vyšší hello méně velká a malá písmena) |rpscore: plovoucí desetinné číslo představující anomálií skóre při změně úrovně směrem nahoru a dolů | |

### <a name="parameters"></a>Parametry
Podrobnější informace o těchto vstupních parametrů je uveden v tabulce hello:

| Vstupní parametry | Popis | Výchozí nastavení | Typ | Platný rozsah | Navržená oblast |
| --- | --- | --- | --- | --- | --- |
| detectors.historyWindow |Historie (v počet datových bodů), které jsou používány pro výpočty anomálií skóre |500 |celé číslo |10-2000 |Závislé na časové řady |
| detectors.spikesdips | Jestli toodetect pouze špičky, pouze vyhrazené IP adresy nebo obojí |Obě |vytvořit její výčet. |Obě špičky, vyhrazené IP adresy |Obě |
| bileveldetector.Sensitivity |Velkých a malých písmen pro úroveň obousměrného změnit detektor. |3.25 |Double |Žádný |3,25-5 (hodnoty menší význam citlivější) |
| trenddetector.Sensitivity |Velkých a malých písmen pro pozitivní trend detektor. |3.25 |Double |Žádný |3,25-5 (hodnoty menší význam citlivější) |
| tspikedetector.Sensitivity |Velkých a malých písmen pro TSpike detektor |3 |celé číslo |1-10 |3 až 5 (hodnoty menší význam citlivější) |
| zspikedetector.Sensitivity |Velkých a malých písmen pro ZSpike detektor |3 |celé číslo |1-10 |3 až 5 (hodnoty menší význam citlivější) |
| postprocess.tailRows |Počet nejnovější data hello body toobe zachovány ve výsledcích výstup hello |0 |celé číslo |0 (uchovávejte všechny datové body), nebo zadejte počet bodů tookeep ve výsledcích |Není k dispozici |

### <a name="output"></a>Výstup
Hello API spustí všechny detektory na datové řady čas a vrátí anomálií skóre a binární Špička indikátory pro každý bod v čase. Následující tabulka Hello uvádí výstupy z hello rozhraní API. 

| Výstupy | Popis |
| --- | --- |
| Čas |Časová razítka z nezpracovaná data nebo data agregované (nebo) imputované Pokud agregace (nebo) chybí data imputace platí |
| Data |Hodnoty z nezpracovaná data nebo data agregované (nebo) imputované Pokud agregace (nebo) chybí data imputace platí |
| TSpike |Binární indikátor tooindicate jestli rozpozná Špička TSpike detektor |
| ZSpike |Binární indikátor tooindicate jestli rozpozná Špička ZSpike detektor |
| rpscore |Při změně úrovně obousměrného stanovení skóre plovoucí desetinné číslo představující anomálií |
| rpalert |Hodnota 1 nebo 0 oznamující, že je úroveň obousměrného změní anomálií podle vstupní citlivosti hello |
| tscore |Plovoucí desetinné číslo představující anomálií stanovení skóre na kladné trendu |
| talert |1 nebo 0 hodnotu udávající, že je kladné trend anomálií, podle vstupní citlivosti hello |

## <a name="scorewithseasonality-api"></a>ScoreWithSeasonality rozhraní API
Hello ScoreWithSeasonality rozhraní API se používá pro spouštění detekce anomálií na časové řady, která sezónní vzorce. Toto rozhraní API je užitečné toodetect odchylky v sezónní vzory.  
Hello následující obrázek znázorňuje příklad anomálie v sezónní časové řady. Hello časové řady má jeden Špička (hello 1 černý bod), dvě vyhrazené IP adresy (hello 2 černý bod a druhý na konci hello) a jeden Změna úrovně (červené tečky). Upozorňujeme, že oba hello dip uprostřed hello hello časové řady a změna úrovně hello jsou discernable pouze po odebrání sezónní součásti z řady hello.
![Sezónnosti rozhraní API][2]

### <a name="detectors"></a>Detektory
detektory Hello v koncovém bodu sezónnosti hello jsou podobné toohello ty, které jsou v jiných sezónnosti hello koncový bod, ale s názvy mírně odlišné parametrů (uvedené níže).

### <a name="parameters"></a>Parametry

Podrobnější informace o těchto vstupních parametrů je uveden v tabulce hello:

| Vstupní parametry | Popis | Výchozí nastavení | Typ | Platný rozsah | Navržená oblast |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Agregace interval v sekundách pro agregaci vstupní časové řady |0 (žádné agregace se provádí) |celé číslo |0: jinak přeskočit agregace > 0 |den too1 5 minut, závislé na časové řady |
| preprocess.aggregationFunc |Funkce použitá pro agregaci dat do hello zadaný AggregationInterval |střední |vytvořit její výčet. |Střední, sum, délka |Není k dispozici |
| preprocess.replaceMissing |Hodnoty použít tooimpute chybějící data |lkv (poslední známá hodnota) |vytvořit její výčet. |nula, lkv, střední |Není k dispozici |
| detectors.historyWindow |Historie (v počet datových bodů), které jsou používány pro výpočty anomálií skóre |500 |celé číslo |10-2000 |Závislé na časové řady |
| detectors.spikesdips | Jestli toodetect pouze špičky, pouze vyhrazené IP adresy nebo obojí |Obě |vytvořit její výčet. |Obě špičky, vyhrazené IP adresy |Obě |
| bileveldetector.Sensitivity |Velkých a malých písmen pro úroveň obousměrného změnit detektor. |3.25 |Double |Žádný |3,25-5 (hodnoty menší význam citlivější) |
| postrenddetector.Sensitivity |Velkých a malých písmen pro pozitivní trend detektor. |3.25 |Double |Žádný |3,25-5 (hodnoty menší význam citlivější) |
| negtrenddetector.Sensitivity |Velkých a malých písmen pro detektor negativní trend. |3.25 |Double |Žádný |3,25-5 (hodnoty menší význam citlivější) |
| tspikedetector.Sensitivity |Velkých a malých písmen pro TSpike detektor |3 |celé číslo |1-10 |3 až 5 (hodnoty menší význam citlivější) |
| zspikedetector.Sensitivity |Velkých a malých písmen pro ZSpike detektor |3 |celé číslo |1-10 |3 až 5 (hodnoty menší význam citlivější) |
| seasonality.enable |Zda je analýza sezónnosti toobe provést |Hodnota TRUE |Logická hodnota |Hodnota TRUE, false |Závislé na časové řady |
| seasonality.numSeasonality |Maximální počet pravidelných cyklů toobe zjistil |1 |celé číslo |1, 2 |1-2 |
| seasonality.Transform |Jestli sezónní (a) trend součásti se musí odebrat před použitím detekce anomálií |deseason |vytvořit její výčet. |NONE, deseason, deseasontrend |Není k dispozici |
| postprocess.tailRows |Počet nejnovější data hello body toobe zachovány ve výsledcích výstup hello |0 |celé číslo |0 (uchovávejte všechny datové body), nebo zadejte počet bodů tookeep ve výsledcích |Není k dispozici |

### <a name="output"></a>Výstup
Hello API spustí všechny detektory na datové řady čas a vrátí anomálií skóre a binární Špička indikátory pro každý bod v čase. Následující tabulka Hello uvádí výstupy z hello rozhraní API. 

| Výstupy | Popis |
| --- | --- |
| Čas |Časová razítka z nezpracovaná data nebo data agregované (nebo) imputované Pokud agregace (nebo) chybí data imputace platí |
| OriginalData |Hodnoty z nezpracovaná data nebo data agregované (nebo) imputované Pokud agregace (nebo) chybí data imputace platí |
| ProcessedData |Buď hello následující: <ul><li>Pokud významné sezónnosti byl zjištěn a deseason možnost; ročních období upravena časové řady</li><li>ročních období upravit a detrendovaného časové řady zjištění významné sezónnosti a vybranou možností deseasontrend</li><li>jinak to je hello stejné jako OriginalData</li> |
| TSpike |Binární indikátor tooindicate jestli rozpozná Špička TSpike detektor |
| ZSpike |Binární indikátor tooindicate jestli rozpozná Špička ZSpike detektor |
| BiLevelChangeScore |Plovoucí desetinné číslo představující anomálií stanovení skóre při změně úrovně |
| BiLevelChangeAlert |1 nebo 0 hodnotu označující, že je změna úrovně anomálií, podle vstupní citlivosti hello |
| PosTrendScore |Plovoucí desetinné číslo představující anomálií stanovení skóre na kladné trendu |
| PosTrendAlert |1 nebo 0 hodnotu udávající, že je kladné trend anomálií, podle vstupní citlivosti hello |
| NegTrendScore |Plovoucí desetinné číslo představující anomálií stanovení skóre na záporné trendu |
| NegTrendAlert |1 nebo 0 hodnotu udávající, že je negativní trend anomálií, podle vstupní citlivosti hello |

[1]: ./media/machine-learning-apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection-api/anomaly-detection-seasonal.png

