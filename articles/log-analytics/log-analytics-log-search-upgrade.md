---
title: "hledání protokolů toonew aaaUpgrading Azure Log Analytics | Microsoft Docs"
description: "Hello nové analýzy protokolů dotazovací jazyk je téměř sem a ve verzi public preview hello se můžete zapojit.  Tento článek popisuje výhody hello hello nový jazyk a jak tooconvert pracovního prostoru."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 7659c9d1467cab79d3a16e73b52b507ed281b002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-azure-log-analytics-workspace-toonew-log-search"></a>Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure

> [!NOTE]
> Upgrade toohello nové analýzy protokolů dotazovací jazyk je aktuálně volitelné poskytnutí času příliš[rychle pochopit práci na nový jazyk hello](https://go.microsoft.com/fwlink/?linkid=856078).  

Hello nové analýzy protokolů dotazovací jazyk je zde a potřebujete tooupgrade vašeho pracovního prostoru tootake jejich výhod.  Tento článek popisuje výhody hello hello nový jazyk a jak tooconvert pracovního prostoru.  Pokud si zvolíte tooupgrade nyní, bude pokračovat pracovního prostoru, toooperate stejně jako se vždy, ale bude automaticky převedena později.  Čas a oznámení obdrží toto datum je nastavena.

Tento článek poskytuje podrobné informace o nový jazyk hello a hello procesu upgradu.

## <a name="why-hello-new-language"></a>Proč hello nový jazyk?
Chápeme, že je problémové v jakékoli přechod a jsme právě nejsou změna hello dotazovacího jazyka pro zábavu hello ho.  Tady je několik důvodů, které tato změna bude poskytovat významné zlepšení tooour analýzy protokolů zákazníků.

- **Jednoduché, ale výkonné.** nový jazyk Hello je snazší toounderstand a podobné tooSQL s konstrukce další jako přirozeného jazyka než starší verze jazyka hello.
- **Úplné zřetězení jazyk.**  nový jazyk Hello má rozsáhlejší zřetězení funkce než pro starší verze jazyka hello.  Prakticky výstup může být vytvoření kanálu tooanother příkaz toocreate složitější dotazy, než bylo dřív možné.
- **Pole pro vyhledávání čas extrakce.**  Hello nový jazyk podporuje pokročilejší runtime počítaného pole než starší verze jazyka hello.  Můžete využít komplexní výpočty pro rozšířené pole a pak pomocí hello počítaného pole pro další příkazy, včetně spojování a agregaci.
- **Pokročilé spojení.**  nový jazyk Hello poskytuje pokročilejší spojení než starší verze jazyka hello včetně hello možnost toojoin tabulky více polí, pomocí vnitřního a vnějšího spojení a připojení na rozšířených polích.
- **Funkce data a času.**  nový jazyk Hello má pokročilejší funkce data a času než starší verze jazyka hello.
- **Inteligentní analýzy.**  Hello nový jazyk má pokročilé algoritmy tooevaluate vzory v datových sad a porovnání různých datových sad.
- **Pokročilé analýzy portálu.**  Hello pokročilou analýzu portál nabízí analytické funkce není k dispozici v portálu Analýza protokolů hello včetně víceřádkových úpravy dotazů, další vizualizace a pokročilé diagnostiky.
- **Konzistence s jinými aplikacemi.**  Hello nový jazyk a hello pokročilé analýzy portál se již používá pro analýzu ve službě Application Insights.  Implementace pro analýzy protokolů poskytuje konzistence napříč službami Azure.
- **Lepší integrace s Power BI.** Dotazy v jazyce nové hello může být exportovaný tooPower BI Desktop, můžete využívat jeho funkce transformace bohaté data.
- **Mnohé další.** Odkazovat toohello [Azure Log Analytics Query Language](https://docs.loganalytics.io) kompletní informace a kurzy o nový jazyk hello lokality.


## <a name="when-can-i-upgrade"></a>Pokud můžete upgradovat?
Hello upgradu bude vrácena přes všechny oblasti Azure, mohou být k dispozici v některé oblasti než jiné.  Budete vědět, pokud pracovní prostor je k dispozici toobe upgradovat, je-li zobrazí banner hello fialové napříč hello horní části pracovního prostoru pozvat jste tooupgrade.

![Upgrade 1](media/log-analytics-log-search-upgrade/upgrade-01a.png)

## <a name="what-happens-when-i-upgrade"></a>Co se stane, když upgraduji?
Při převodu pracovního prostoru, všechny uložená hledání, pravidla výstrah a zobrazení, které jste vytvořili pomocí hello Návrhář zobrazení jsou automaticky převedený toohello nový jazyk.  Hledání součástí řešení se nepřevádějí automaticky, ale budou se místo toho převést na hello chodu při jejich otevření.  Toto je zcela transparentní tooyou.

## <a name="can-i-go-back-after-i-upgrade"></a>Můžete přejít zpět po upgradu?
Při upgradu, se provede úplné zálohy pracovního prostoru, který obsahuje snímek všech uložená hledání, pravidlo výstrahy a zobrazení.  To vám umožní toorestore vaše staré prostoru by měl později vyžadujete-li.

toorestore pracovního prostoru starší verze přejděte příliš**nastavení** v pracovním prostoru a potom **upgradu Souhrn**.  Pak můžete vybrat možnost hello příliš**obnovit starší verze prostoru**.  

![Obnovit starší verze](media/log-analytics-log-search-upgrade/restore-legacy-b.png)

## <a name="how-do-i-perform-hello-upgrade"></a>Jak se provádí hello upgrade?
Pracovní prostor můžete upgradovat až uvidíte hello fialové hlavičky v horní části hello hello portálu.  

1.  Spuštění procesu upgradu hello kliknutím na hello fialové informační zpráva, která uvádí, že **Další informace a upgradujte**.<br>![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>
2.  Pročtěte hello Další informace o upgradu hello na stránce informace o upgradu hello.<br>![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>
3.  Klikněte na tlačítko **upgradovat nyní** toostart hello upgradu.<br>![Upgrade 4](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>Pole oznámení v pravém horním rohu hello se zobrazuje stav hello.<br>![Upgrade 5](media/log-analytics-log-search-upgrade/upgrade-05.png)
4.  A to je vše!  Projít toohello hledání protokolů stránky toohave podívejte se na pracovní prostor upgradovaný.<br>![Upgrade 6](media/log-analytics-log-search-upgrade/upgrade-06.png)<br>

Pokud narazíte na problém, který způsobuje hello upgradu toofail, můžete přejít toohello [diskusní fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) a vystavte tady svůj dotaz nebo [vytvořit žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md) z hello portálu Azure.

## <a name="how-do-i-learn-hello-new-language"></a>Jak další hello nový jazyk?
Vzhledem k tomu, že se používají v několika služeb vytvořili jsme [externího webu toohost hello dokumentaci](https://docs.loganalytics.io/) pro nový jazyk hello.  To zahrnuje kurzy, ukázky a toohelp úplný odkaz, který vytvoříte toospeed. Si můžete projít kurz hello nového jazyka v [Začínáme s dotazy](https://go.microsoft.com/fwlink/?linkid=856078) a přístup k referenční informace k jazyku hello v [analýzy protokolů dotazu jazyka](https://go.microsoft.com/fwlink/?linkid=856079).  

Pokud jste již obeznámeni s hello starší verze protokolu Analytics query language, když pak můžete použít převaděč hello jazyk, který se přidal tooyour prostoru jako součást upgradu hello.

Jednoduše zadejte starší verze dotaz a pak klikněte na tlačítko **převést** toosee hello přeložit verze.  Můžete buď klikněte na tlačítko hello vyhledávání tlačítko toorun hello hledání nebo kopírování a vkládání hello převést dotazu toouse někde jinde, například pravidlo výstrahy.

![Převaděč jazyk](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Další kroky
- Podívejte se [kurz na nový jazyk hello](https://go.microsoft.com/fwlink/?linkid=856078).
- Provede [kurz na portálu hledání protokolů hello](log-analytics-log-search-log-search-portal.md) s hello nové dotazovací jazyk.
- Získat nové Úvod toohello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587).
