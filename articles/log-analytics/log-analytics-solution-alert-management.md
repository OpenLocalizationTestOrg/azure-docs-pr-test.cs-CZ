---
title: "aaaAlert řešení pro správu v Operations Management Suite (OMS) | Microsoft Docs"
description: "Hello řešení pro správu výstrah v analýzy protokolů pomáhá analyzovat všechna hello výstrahy ve vašem prostředí.  V přidání tooconsolidating výstrahy generované v rámci OMS se importuje výstrahy z připojených skupin pro správu System Center Operations Manager do analýzy protokolů."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: aff9bd8d88839c5227bb9ec3a1b5209a3cd7cdf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Výstrahy řešení pro správu v Operations Management Suite (OMS)

![Ikona správy výstrah](media/log-analytics-solution-alert-management/icon.png)

Hello řešení pro správu výstrah pomáhá analyzovat všechna hello výstrahy do úložiště analýzy protokolů.  Tyto výstrahy mohou mít pocházet z různých zdrojů, včetně těchto zdrojů [vytvořené analýzy protokolů](log-analytics-alerts.md) nebo [importovat z Nagios nebo Zabbix](log-analytics-linux-agents.md).  Hello řešení také importuje výstrahy z libovolného [připojené skupiny pro správu System Center Operations Manager](log-analytics-om-agents.md).

## <a name="prerequisites"></a>Požadavky
řešení Hello funguje se všechny záznamy v hello analýzy protokolů úložiště s typem **výstrahy**, takže je třeba provést, ať konfigurace je požadovaná toocollect tyto záznamy.

- Pro analýzy protokolů výstrahy [vytvořit pravidla výstrah](log-analytics-alerts.md) toocreate výstrahy záznamy přímo v úložišti hello.
- Pro výstrahy Nagios a Zabbix [konfiguraci těchto serverů](log-analytics-linux-agents.md) toosend výstrahy tooLog Analytics.
- Pro System Center Operations Manager výstrahy [připojit vaše nástroje Operations Manager management skupiny tooyour pracovní prostor analýzy protokolů](log-analytics-om-agents.md).  Všechny výstrahy vytvořené v nástroji System Center Operations Manager jsou importovány do analýzy protokolů.  

## <a name="configuration"></a>Konfigurace
Přidat tooyour řešení pro správu výstrah hello pracovním prostorem OMS pomocí procesu hello popsané v [přidat řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

## <a name="management-packs"></a>Sady Management Pack
Pokud skupině pro správu System Center Operations Manager je pracovní prostor OMS připojené tooyour, jsou při přidání tohoto řešení hello následující sady management Pack nainstalované v System Center Operations Manager.  Neexistuje žádná konfigurace ani údržby sad management Pack hello vyžaduje.  

* Microsoft System Center Advisor správu výstrah (Microsoft.IntelligencePacks.AlertManagement)

Další informace o tom, jak jsou aktualizovány sady management Pack řešení najdete v tématu [tooLog připojení nástroje Operations Manager Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Shromažďování dat
### <a name="agents"></a>Agenti
Hello následující tabulka popisuje hello připojené zdroje, které podporuje toto řešení.

| Připojený zdroj | Podpora | Popis |
|:--- |:--- |:--- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne |Přímé agenty se systémem Windows negenerují výstrahy.  Log Analytics výstrahy lze vytvořit na základě události a údaje o výkonu shromážděných ze systému Windows agenty. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne |Přímé agenty Linux negenerují výstrahy.  Log Analytics výstrahy mohou být vytvořeny z události a data výkonu shromážděných z agentů systému Linux.  Z těchto serverů, které vyžadují agenta systému Linux hello se shromažďují Nagios a Zabbix výstrahy. |
| [Skupina pro správu System Center Operations Manager](log-analytics-om-agents.md) |Ano |Výstrahy, které jsou vytvořeny na agenty nástroje Operations Manager jsou doručeny toohello skupiny pro správu a pak předávaných tooLog Analytics.<br><br>Přímé připojení z nástroje Operations Manager agenty tooLog Analytics se nevyžaduje. Data výstrah se předají z hello správy skupiny toohello analýzy protokolů úložiště. |


### <a name="collection-frequency"></a>Četnost shromažďování dat
- Výstrahy záznamy jsou k dispozici toohello řešení, jakmile jsou uložené v úložišti hello.
- Výstrahy odeslání dat ze hello nástroje Operations Manager management skupiny tooLog Analytics každé tři minuty.  

## <a name="using-hello-solution"></a>Pomocí řešení hello
Když přidáte hello správu výstrah řešení tooyour pracovním prostorem OMS, hello **správu výstrah** dlaždice se přidá tooyour OMS řídicího panelu.  Tuto dlaždici zobrazí počet a grafické znázornění hello počet aktuálně aktivních výstrah, které byly vygenerovány v rámci hello posledních 24 hodin.  Tento časový rozsah nelze změnit.

![Dlaždice výstrah správy](media/log-analytics-solution-alert-management/tile.png)

Klikněte na hello **správu výstrah** dlaždice tooopen hello **správu výstrah** řídicího panelu.  řídicí panel Hello obsahuje sloupce hello hello následující tabulka.  Každý sloupec uvádí hello top 10 výstrahy podle odpovídající počet, sloupce kritéria pro hello zadaný rozsah oboru a času.  Můžete spustit hledání protokolu, které poskytuje celý seznam hello kliknutím **zobrazit všechny** dolnímu hello hello sloupce nebo kliknutím na záhlaví sloupce hello.

| Sloupec | Popis |
|:--- |:--- |
| Kritické výstrahy |Všechny výstrahy se závažností kritický seskupené podle název výstrahy.  Klikněte na název výstrahy toorun vyhledávání protokolu vrátit všechny záznamy pro tuto výstrahu. |
| Varovné výstrahy. |Všechny výstrahy se závažností upozornění seskupené podle název výstrahy.  Klikněte na název výstrahy toorun vyhledávání protokolu vrátit všechny záznamy pro tuto výstrahu. |
| Aktivní SCOM výstrahy |Všechny výstrahy shromážděné z nástroje Operations Manager pomocí nějaký stav, jiné než *uzavřeno* seskupené podle zdroje výstrahy generované hello. |
| Všechny aktivní výstrahy |Všechny výstrahy s všechny závažnosti seskupené podle název výstrahy. Zahrnuje výstrahy nástroje Operations Manager s jakýkoli stav jenom jiné než *uzavřeno*. |

Pokud se posunete toohello práva, řídicí panel hello uvádí několik běžných dotazů, které můžete kliknutím na tooperform [hledání protokolů](log-analytics-log-searches.md) pro dat výstrah.

![Výstrahy řídicí panel správy](media/log-analytics-solution-alert-management/dashboard.png)


## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Hello řešení pro správu výstrah analyzuje všech záznamů s typem **výstrahy**.  Výstrahy vytvořené analýzy protokolů nebo shromážděných ze Nagios nebo Zabbix přímo neshromáždí hello řešení.

řešení Hello importovat výstrahy z nástroje System Center Operations Manager a vytvoří odpovídající záznam pro každý typ **výstrahy** a SourceSystem z **OpsManager**.  Tyto záznamy mají vlastnosti hello v hello následující tabulka:  

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*Výstrahy* |
| SourceSystem |*OpsManager* |
| AlertContext |Podrobnosti o hello datová položka, která způsobila výstrahu toobe hello generované ve formátu XML. |
| AlertDescription |Podrobný popis výstrahy hello. |
| AlertId |Identifikátor GUID hello výstrahy. |
| AlertName |Název výstrahy hello. |
| AlertPriority |Úroveň priority výstrahy hello. |
| AlertSeverity |Úroveň závažnosti výstrahy hello. |
| AlertState |Nejnovější stav řešení výstrahy hello. |
| LastModifiedBy |Název hello uživatele, který naposledy upravil hello výstrahy. |
| ManagementGroupName |Název skupiny pro správu hello kde hello výstrahu vygenerovalo. |
| RepeatCount |Počet opakování pro stejný objekt monitorovat od přeloženy hello hello stejnou výstrahu vygenerovalo. |
| ResolvedBy |Název hello uživatele, který rozpoznána hello výstraha. Prázdný, pokud hello výstraha dosud nebyl vyřešen. |
| SourceDisplayName |Zobrazovaný název hello objekt, který generoval výstrahu hello monitorování. |
| SourceFullName |Celý název hello objekt, který generoval výstrahu hello monitorování. |
| TicketId |ID lístku pro hello výstrahu v případě, že prostředí pro System Center Operations Manager hello je integrovaná s procesem pro přidělování lístků pro výstrahy.  Prázdný žádné lístku ID je přiřazen. |
| TimeGenerated |Datum a čas, který hello výstraha byla vytvořena. |
| TimeLastModified |Datum a čas, který hello výstraha byl naposled změněn. |
| TimeRaised |Datum a čas, který hello výstrahu vygenerovalo. |
| TimeResolved |Datum a čas, který hello výstraha byl vyřešen. Prázdný, pokud hello výstraha dosud nebyl vyřešen. |

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech
Hello následující tabulka obsahuje ukázkový protokol hledání výstrah záznamů shromažďují toto řešení: 

| Dotaz | Popis |
|:--- |:--- |
| Typ = výstrahy SourceSystem = OpsManager AlertSeverity = chyby TimeRaised > nyní 24 hodin |Kritické výstrahy vyvolané během hello posledních 24 hodin |
| Typ = výstrahy AlertSeverity = upozornění TimeRaised > nyní 24 hodin |Upozorňující výstrahy vyvolané během hello posledních 24 hodin |
| Typ = výstrahy SourceSystem = OpsManager AlertState! = uzavřené TimeRaised > nyní 24 hodin &#124; míra count() jako počet podle SourceDisplayName |Zdroje s aktivními výstrahami vyvolanými během hello posledních 24 hodin |
| Typ = výstrahy SourceSystem = OpsManager AlertSeverity = chyby TimeRaised > nyní 24 hodin AlertState! = uzavřeno |Kritické výstrahy vyvolané během hello posledních 24 hodin, které jsou stále aktivní |
| Typ = výstrahy SourceSystem = OpsManager TimeRaised > nyní 24 hodin AlertState = uzavřený |Výstrahy vyvolané během hello posledních 24 hodin, které už jsou uzavřené |
| Typ = výstrahy SourceSystem = OpsManager TimeRaised > nyní - 1 den &#124; míra count() jako počet podle AlertSeverity |Výstrahy vyvolané během hello poslední 1 den seskupené podle závažnosti |
| Typ = výstrahy SourceSystem = OpsManager TimeRaised > nyní - 1 den &#124; řazení RepeatCount desc |Výstrahy vyvolané za poslední 1 den seřazené podle počtu opakování hello |


>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello předcházející dotazy změní toohello následující:
>
>| Dotaz | Popis |
|:---|:---|
| Upozornit &#124; kde SourceSystem == "OpsManager" a AlertSeverity == "Chyba" a TimeRaised > ago(24h) |Kritické výstrahy vyvolané během hello posledních 24 hodin |
| Upozornit &#124; kde AlertSeverity == "upozornění" a TimeRaised > ago(24h) |Upozorňující výstrahy vyvolané během hello posledních 24 hodin |
| Upozornit &#124; kde SourceSystem == "OpsManager" a AlertState! = "Uzavřeno" a TimeRaised > ago(24h) &#124; shrnout Count = count() podle SourceDisplayName |Zdroje s aktivními výstrahami vyvolanými během hello posledních 24 hodin |
| Upozornit &#124; kde SourceSystem == "OpsManager" a AlertSeverity == "Chyba" a TimeRaised > ago(24h) a AlertState! = "Uzavřeno" |Kritické výstrahy vyvolané během hello posledních 24 hodin, které jsou stále aktivní |
| Upozornit &#124; kde SourceSystem == "OpsManager" a TimeRaised > ago(24h) a AlertState == "Uzavřeno" |Výstrahy vyvolané během hello posledních 24 hodin, které už jsou uzavřené |
| Upozornit &#124; kde SourceSystem == "OpsManager" a TimeRaised > ago(1d) &#124; shrnout Count = count() podle AlertSeverity |Výstrahy vyvolané během hello poslední 1 den seskupené podle závažnosti |
| Upozornit &#124; kde SourceSystem == "OpsManager" a TimeRaised > ago(1d) &#124; Řadit podle RepeatCount desc |Výstrahy vyvolané za poslední 1 den seřazené podle počtu opakování hello |


## <a name="next-steps"></a>Další kroky
* Podrobnosti o generování upozornění ze služby Log Analytics najdete v tématu [Upozornění v Log Analytics](log-analytics-alerts.md).
