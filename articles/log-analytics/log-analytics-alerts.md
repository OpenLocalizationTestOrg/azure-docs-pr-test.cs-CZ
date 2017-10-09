---
title: "výstrahy aaaUnderstanding v Azure Log Analytics | Microsoft Docs"
description: "Výstrahy v analýzy protokolů zjišťovat důležité informace ve svém úložišti OMS a zda lze proaktivně upozorňují na problémy nebo vyvolání akce tooattempt toocorrect je.  Tento článek popisuje hello různé druhy pravidla výstrah a jak jsou definovány."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: bfa0a5aaeca81674e79a6d647f36d937efeeb439
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-alerts-in-log-analytics"></a>Vysvětlení výstrah v analýzy protokolů

Výstrahy v Log Analytics identifikovat důležité informace do úložiště analýzy protokolů.  Tento článek obsahuje podrobnosti o tom, jak výstrahy pravidlech pracovního analýzy protokolů a popisuje hello rozdíly mezi různými druhy pravidla výstrah.

Hello proces vytváření pravidla výstrah najdete v části hello následující články:

- Vytvořit pravidla výstrah pomocí [portálu Azure](log-analytics-alerts-creating.md)
- Vytvořit pravidla výstrah pomocí [šablony Resource Manageru](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Vytvořit pravidla výstrah pomocí [REST API](log-analytics-api-alerts.md)


## <a name="alert-rules"></a>Pravidla výstrah

Výstrahy jsou vytvářeny pravidla výstrah, které automaticky spustit vyhledávání protokolu v pravidelných intervalech.  Pokud hello výsledky hledání protokolů hello odpovídají konkrétním kritériím se vytvoří záznam výstrahy.  pravidlo Hello potom automaticky spustit jeden nebo další akce tooproactively oznámíme vám hello výstraha nebo vyvolat jiný proces.  Různé typy pravidla výstrah pomocí různých logiku tooperform této analýze.

![Výstrahy Log Analytics](media/log-analytics-alerts/overview.png)

Pravidla výstrah jsou definovány hello následující podrobnosti:

- **Hledání protokolů**.  Hello dotaz, který se spustí pokaždé, když se aktivuje hello pravidlo výstrahy.  vrácené tímto dotazem záznamy Hello je použité toodetermine, zda se vytvoří výstraha.
- **Časový interval**.  Určuje hello časový rozsah pro dotaz hello.  Hello dotaz vrátí pouze záznamy, které byly vytvořeny v tomto rozsahu hello aktuální čas.  To může být libovolná hodnota 5 minut až 24 hodin. Například pokud hello okno bude nastaveno too60 minut a hello dotazů, když se spustí: 15: 00, je vrácena pouze záznamy vytvořené 12:15:00 až 1:15 hodin.
- **Frekvence**.  Určuje, jak často hello dotaz by měl být spuštěn. Může být libovolná hodnota 5 minut až 24 hodin. Musí být rovna tooor menší než hello časový interval.  Pokud hodnota hello je větší než hello časový interval, riskujete záznamů je vynechán.<br>Představte si třeba časové okno 30 minut a četnost 60 minut.  Pokud dotaz hello běží v 1:00, vrátí záznamy 12:30 až 1:00 PM.  Hello by spuštění dotazu hello je 2:00, když měla by vrátit záznamy 1:30 až 2:00.  Všechny záznamy vytvořené 1:00 až 1:30 by nikdy vyhodnotí.
- **Prahová hodnota**.  Hello výsledky hledání protokolů hello jsou toodetermine vyhodnotí, zda má být vytvořena výstraha.  Prahová hodnota Hello se liší pro různé typy hello pravidla výstrah.

Každé pravidlo výstrahy v analýzy protokolů je jedním ze dvou typů.  Každý z těchto typů je podrobně popsány v následujících hello částech.

- **[Počet výsledků](#number-of-results-alert-rules)**. Jedna výstraha vytvoří, když hello počet záznamů vrácených hledání protokolů hello překročit určeného čísla.
- **[Metriky měření](#metric-measurement-alert-rules)**.  Výstraha byla vytvořena pro každý objekt v hello výsledky hledání protokolů hello s hodnotami, které překročí zadanou prahovou hodnotu.

Hello rozdíly mezi typy pravidlo výstrahy se následujícím způsobem.

- **Počet výsledků** pravidlo výstrahy vytvoří vždy jeden výstrahy chvíli **metriky měření** pravidlo výstrahy vytvoří výstrahu pro každý objekt, který překračuje prahovou hodnotu hello.
- **Počet výsledků** pravidla výstrah vytvářejí výstrahu při překročení prahové hodnoty hello současně. **Metriky měření** pravidla výstrah může vytvořit upozornění, když je prahová hodnota hello počet časy v konkrétním časovém intervalu.

## <a name="number-of-results-alert-rules"></a>Počet výsledků pravidla výstrah
**Počet výsledků** pravidla výstrah vytvořit jednu výstrahu, když hello počet záznamů vrácených dotazem vyhledávání hello překročí zadanou prahovou hodnotu hello.

### <a name="threshold"></a>Prahová hodnota
Prahová hodnota Hello **počet výsledků** pravidlo výstrahy je jednoduše větší nebo menší než určitou hodnotu.  Pokud hello počet záznamů vrácených hello protokolu vyhledávání shody tato kritéria, se vytvoří výstraha.

### <a name="scenarios"></a>Scénáře

#### <a name="events"></a>Události
Tento typ pravidla výstrah je ideální pro práci s událostmi, jako je například protokol událostí systému Windows, Syslog, a vlastní protokoly.  Můžete chtít toocreate výstrahu, když se vytvoří událost konkrétní chyby, nebo když jsou v rámci konkrétní časové okno vytvořit více událostí chyby.

tooalert na jednu událost, nastavte hello počet výsledků toogreater než 0 a obě hello četnost a čas okno too5 minut.  Spuštění dotazu hello každých 5 minut a kontrola hello výskytu jedné události, který byl vytvořen, protože byl spuštěn hello poslední čas hello dotaz.  Delší frekvence může zdržet hello čas mezi hello událost vrácení shromážděných a vytváří výstrahy hello.

Některé aplikace mohou být zaznamenány občasné chyby, který by neměl být nutně vygeneruje výstrahu.  Například může aplikace hello opakujte hello procesu, který vytvořil hello chybová událost a potom úspěšné hello příště.  V takovém případě nemusí chcete výstrahu hello toocreate Pokud jsou v rámci konkrétní časové okno vytvořit více událostí.  

V některých případech můžete toocreate výstrahu hello neexistence událost.  Tento proces se může například protokolu tooindicate běžné události, který pracuje správně.  Pokud není protokolu jednu z těchto událostí v rámci konkrétní časové okno, je třeba vytvořit výstrahu.  V takovém případě byste měli nastavit prahovou hodnotu hello příliš**menší než 1**.

#### <a name="performance-alerts"></a>Výstrahy výkonu
[Data výkonu](log-analytics-data-sources-performance-counters.md) se ukládají jako záznamy v hello podobné tooevents OMS úložiště.  Pokud chcete tooalert, pokud čítače výkonu překročí konkrétní prahovou hodnotu, pak prahové hodnoty by měl být součástí hello dotazu.

Například pokud byste chtěli tooalert při hello procesoru, které se spouští 90 % byste použili dotazu jako hello následující s hello prahová hodnota pro pravidlo výstrahy hello **větší než 0**.

    Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90

Pokud byste chtěli tooalert při hello procesoru průměrem více než 90 % pro konkrétní časové okno, byste použili dotazu pomocí hello [měření příkaz](log-analytics-search-reference.md#commands) jako hello pomocí hello prahová hodnota pro pravidlo výstrahy hello **větší než 0.** .

    Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello výše dotazy změní toohello následující:`Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90`
> `Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | summarize avg(CounterValue) by Computer | where CounterValue>90`


## <a name="metric-measurement-alert-rules"></a>Pravidla výstrah metriky měření

>[!NOTE]
> Pravidla výstrah metriky měření jsou aktuálně ve verzi public preview.

**Metriky měření** pravidla výstrah vytvářejí výstrahu pro každý objekt v dotazu s hodnotou, který je delší než zadaná prahová hodnota.  Mají hello následující významné rozdíly z **počet výsledků** pravidla výstrah.

#### <a name="log-search"></a>Prohledávání protokolů
I když můžete používat pro jakýkoli dotaz **počet výsledků** pravidlo výstrahy, jsou konkrétní požadavky hello dotazu pro pravidlo výstrahy metriky měření.  Musí obsahovat [měření příkaz](log-analytics-search-reference.md#commands) toogroup hello výsledky podle určitého pole. Tento příkaz musí zahrnovat následující prvky hello.

- **Agregační funkce**.  Určuje hello výpočet, který provádí a potenciálně tooaggregate číselné pole.  Například **count()** vrátí hello počet záznamů v dotazu hello **avg(CounterValue)** vrátí průměr hello hello přepočtené pole průběhu hello intervalu.
- **Pole Seskupit**.  Pro každou instanci v tomto poli se vytvoří záznam s hodnotou agregované a výstraha může vygenerovat pro každý.  Například pokud byste chtěli toogenerate výstrahu pro každý počítač, byste použili **počítačem**.   
- **Interval**.  Definuje hello časový interval, za které agregovaná hello data.  Například pokud jste zadali **5minutes**, by se vytvoří záznam pro každou instanci pole skupiny hello v hello časový interval zadaný pro výstrahu hello agregován v intervalech 5 minut.

#### <a name="threshold"></a>Prahová hodnota
Hello prahová hodnota pro pravidla výstrah metriky měření je definována agregovaná hodnota a celou řadu.  Pokud žádné datového bodu v hledání protokolů hello překročí tuto hodnotu, považuje za porušení.  Pokud hello počet porušení v u všech objektů ve výsledcích hello překročí hello zadali hodnotu a potom pro tento objekt se vytvoří výstraha.

#### <a name="example"></a>Příklad
Vezměte v úvahu scénář, kde jste chtěli výstrahu překračování libovolného počítače využití procesoru 90 % třikrát více než 30 minut.  S hello následující podrobnosti by vytvořit pravidlo výstrahy.  

**Dotaz:** typ = výkonu ObjectName = název_čítače procesoru = "% času procesoru" | měření avg(CounterValue) počítače interval 5 minut<br>
**Časový interval:** 30 minut<br>
**Frekvence výstrah:** 5 minut<br>
**Agregace hodnota:** skvělé než 90<br>
**Aktivační událost upozornění na základě:** celkem poruší větší než 5<br>

Hello dotazu by vytvořit průměrnou hodnotu pro každý počítač v intervalech 5 minut.  Tento dotaz by spustit každých 5 minut pro data shromážděná prostřednictvím hello předchozí 30 minut.  Ukázková data jsou uvedené dole pro tři počítače.

![Ukázkové výsledky dotazu](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

V tomto příkladu by se vytvořily samostatné výstrahy pro srv02 a srv03 vzhledem k tomu, že prahová hodnota 90 % hello nedodržení 3krát přes hello časový interval.  Pokud hello **aktivační událost upozornění na základě:** byly změněny příliš**za sebou** výstrahu by vytvořen pouze pro srv03, od nichž nebyla dodržena prahová hodnota hello 3 po sobě jdoucích vzorků.

## <a name="alert-records"></a>Výstrahy záznamů
Mít výstrahy záznamy vytvořené pravidla výstrah v analýzy protokolů **typ** z **výstraha** a **SourceSystem** z **OMS**.  V následující tabulce hello mají vlastnosti hello.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*Výstrahy* |
| SourceSystem |*OMS* |
| *Objekt*  | [Metriky měření výstrahy](#metric-measurement-alert-rules) bude mít vlastnost pro pole skupiny hello.  Například pokud hello protokolu hledání skupin v počítači, hello výstrahy záznam s mít počítač pole s názvem hello hello počítače jako hodnota hello.
| AlertName |Název výstrahy hello. |
| AlertSeverity |Úroveň závažnosti výstrahy hello. |
| LinkToSearchResults |Odkaz tooLog Analytics protokolu vyhledávání, který vrátí hello záznamů z hello dotazu, který vytvořili výstrahu hello. |
| Dotaz |Text dotazu hello, která byla spuštěna. |
| QueryExecutionEndTime |Konec hello časový rozsah pro dotaz hello. |
| QueryExecutionStartTime |Začátek hello časový rozsah pro dotaz hello. |
| ThresholdOperator | Operátor, který byl používán hello pravidlo výstrahy. |
| ThresholdValue | Hodnota, která byla používána pro pravidlo výstrahy hello. |
| TimeGenerated |Datum a čas hello výstraha byla vytvořena. |

Existují jiné typy záznamů výstrahy vytvořené hello [řešení pro správu výstrahu](log-analytics-solution-alert-management.md) a [Power BI exportuje](log-analytics-powerbi.md).  Tyto jsou vybavené **typ** z **výstrahy** , ale jsou rozlišené jejich **SourceSystem**.


## <a name="next-steps"></a>Další kroky
* Nainstalujte hello [řešení pro správu výstrah](log-analytics-solution-alert-management.md) tooanalyze výstrahy vytvořené v analýzy protokolů společně s výstrahy shromážděných z System Center Operations Manager.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) , mohou generovat výstrahy.
* Dokončete průvodce pro [konfigurace webook](log-analytics-alerts-webhooks.md) s pravidlo výstrahy.  
* Zjistěte, jak toowrite [sady runbook ve službě Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problémy identifikovat pomocí výstrah.
