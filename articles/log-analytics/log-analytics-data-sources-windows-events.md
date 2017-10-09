---
title: "aaaCollect a analyzovat protokoly událostí systému Windows v OMS Log Analytics | Microsoft Docs"
description: "Protokoly událostí systému Windows jsou jedním z nejčastějších zdroje dat hello používané analýzy protokolů.  Tento článek popisuje, jak tooconfigure shromažďování protokolů událostí systému Windows a podrobnosti záznamů hello vytvoří v úložišti OMS hello."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2017
ms.author: bwren
ms.openlocfilehash: c05648af39258443f22fd11e1d751b5ccec8c391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Zdroje dat protokolu událostí systému Windows v analýzy protokolů
Protokoly událostí systému Windows jsou jedním z nejčastějších hello [zdroje dat](log-analytics-data-sources.md) pro shromažďování dat pomocí agentů v systému Windows, vzhledem k tomu, že řada aplikací pro zápis toohello protokolu událostí systému Windows.  Může shromažďovat události z standardní protokoly, jako je například systém a aplikace v přidání toospecifying žádné vlastní protokoly vytvořit aplikace potřebujete toomonitor.

![Události systému Windows](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Protokoly událostí konfigurace systému Windows
Konfigurace protokolů událostí systému Windows z hello [nabídce Data v nastavení analýzy protokolů](log-analytics-data-sources.md#configuring-data-sources).

Analýzy protokolů pouze shromažďuje události z protokolů událostí systému Windows hello, které jsou určené v nastavení hello.  Protokol událostí můžete přidat zadáním v hello název hello protokolu a kliknutím na tlačítko  **+** .  Pro každý protokol se shromažďují pouze hello události s hello vybrané závažnosti.  Zkontrolujte hello závažnosti pro hello konkrétní protokolu, které chcete toocollect.  Žádné další kritéria nemůžete zadat toofilter události.

Jak budete zadávat hello název protokolu událostí, analýzy protokolů poskytuje návrhy běžné názvy protokolu událostí. Pokud chcete, aby tooadd protokolu hello se nezobrazí v seznamu hello, je stále přidat ji tak, že zadáte v hello úplný název hello protokolu. Úplný název hello hello protokolu můžete najít pomocí prohlížeče událostí. V prohlížeči událostí, otevřete hello *vlastnosti* stránku hello protokolu a zkopírujte řetězec hello z hello *úplný název* pole.

![Konfigurace události systému Windows](media/log-analytics-data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Shromažďování dat
Analýzy protokolů shromažďuje všechny události, která odpovídá vybrané závažnost z monitorovaných protokolu událostí při vytváření událostí hello.  Hello agent zaznamenává jeho místo v každém protokolu událostí, shromažďující z.  Pokud agenta hello přejde do režimu offline dobu, pak analýzy protokolů shromažďuje události od poslední místa vypnout, i v případě, že tyto události byly vytvořeny v době, kdy hello agent offline.  Pro tyto události toonot shromáždit, pokud protokol událostí hello zabalí s nesebraný události přepsáním při offline hello agenta, existuje možnost.

>[!NOTE]
>Analýzy protokolů nejsou shromažďovány události auditu vytvořené systémem SQL Server ze zdroje *MSSQLSERVER* s ID události 18453, který obsahuje klíčová slova - *Classic* nebo *auditovat úspěšné* a – klíčové slovo *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Vlastnosti záznamů událostí systému Windows
Záznamy událostí Windows mít typ **událostí** a mít hello vlastnosti v hello následující tabulka:

| Vlastnost | Popis |
|:--- |:--- |
| Počítač |Název hello počítače, který hello událost nebyla shromážděna z. |
| EventCategory |Kategorie události hello. |
| EventData |Data všech událostí v nezpracovaném formátu. |
| ID události |Počet událostí hello. |
| eventLevel |Závažnost události hello v číselný tvar. |
| EventLevelName |Závažnost události hello v textové podobě. |
| Protokol událostí |Název protokolu událostí hello hello událost nebyla shromážděna z. |
| ParameterXml |Hodnoty parametru události ve formátu XML. |
| ManagementGroupName |Název skupiny pro správu hello agenty nástroje System Center Operations Manager.  Pro jiné agenty tato hodnota je AOI-<workspace ID> |
| RenderedDescription |Popis události s hodnotami parametrů |
| Zdroj |Zdroj události hello. |
| SourceSystem |Typ události hello agenta nebyla shromážděna z. <br> Připojit OpsManager – agent systému Windows, buď přímo nebo spravovaná nástroje Operations Manager <br> Linux – všechny agenty Linux  <br> Azurestorage – Azure Diagnostics. |
| TimeGenerated |Datum a čas hello událost byla vytvořena v systému Windows. |
| Uživatelské jméno |Uživatelské jméno účtu hello, které protokoluje události hello. |

## <a name="log-searches-with-windows-events"></a>Protokol hledání se události systému Windows
Hello následující tabulka obsahuje různé příklady vyhledávání protokolu, které načtení záznamů událostí systému Windows.

| Dotaz | Popis |
|:--- |:--- |
| Typ = událostí |Všechny události systému Windows. |
| Typ = událostí EventLevelName = chyby |Všechny události systému Windows s závažnosti chyby. |
| Typ = událostí &#124; Míra count() zdrojem. |Události počet Windows zdrojem. |
| Typ = událostí EventLevelName = chyby &#124; Míra count() zdrojem. |Události chyb počet Windows zdrojem. |


>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak změní následující toohello hello výše dotazy.
>
>| Dotaz | Popis |
|:---|:---|
| Událost |Všechny události systému Windows. |
| Událost &#124; kde EventLevelName == "error. |Všechny události systému Windows s závažnosti chyby. |
| Událost &#124; shrnout count() zdrojem. |Události počet Windows zdrojem. |
| Událost &#124; kde EventLevelName == "Chyba" &#124; shrnout count() zdrojem. |Události chyb počet Windows zdrojem. |


## <a name="next-steps"></a>Další kroky
* Konfigurace analýzy protokolů toocollect jiných [zdroje dat](log-analytics-data-sources.md) pro analýzu.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.  
* Použití [vlastní pole](log-analytics-custom-fields.md) tooparse hello záznamů událostí do jednotlivých polí.
* Konfigurace [kolekci čítačů výkonu](log-analytics-data-sources-performance-counters.md) z agentů v systému Windows.
