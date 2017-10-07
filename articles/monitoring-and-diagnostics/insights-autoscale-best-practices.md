---
title: "aaaBest postupy pro škálování | Microsoft Docs"
description: "Vzory škálování v Azure Web Apps, sady škálování virtuálního počítače a cloudové služby"
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 9fa2b94b-dfa5-4106-96ff-74fd1fba4657
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: ancav
ms.openlocfilehash: eb731c15e440af93a2675210583878814d0d8818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-autoscale"></a>Osvědčené postupy pro automatické škálování
V tomto článku se dozvíte, jaké osvědčené postupy tooautoscale v Azure. Škálování Azure monitorování platí pouze příliš[sady škálování virtuálního počítače](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [cloudové služby](https://azure.microsoft.com/services/cloud-services/), a [služby App Service – webové aplikace](https://azure.microsoft.com/services/app-service/web/). Jinými službami Azure použít různé metody škálování.

## <a name="autoscale-concepts"></a>Koncepty škálování
* Prostředek může mít pouze *jeden* nastavení automatického škálování
* Nastavení automatického škálování může mít jeden nebo více profilů a každý profil může mít jeden nebo více pravidel škálování.
* Nastavení automatického škálování škáluje instance vodorovně, což je *out* zvýšením hello instancí a *v* snížením hello počet instancí.
  Nastavení automatického škálování má maximální, minimální a výchozí hodnota instancí.
* Úloha automatického škálování vždy přečte hello spojené metriky tooscale kontrolou, pokud překročila nakonfigurovanou prahovou hodnotu hello Škálováním na více systémů nebo škálování. Můžete zobrazit seznam metrik této škálování můžete škálovat podle na [běžné metriky automatického škálování Azure monitorování](insights-autoscale-common-metrics.md).
* Všechny prahové hodnoty jsou vypočítávány na úrovni instance. Například "škálování odhlašování 1 instancí při průměrná procesoru > 80 %, pokud je počet instancí 2", znamená Škálováním na více systémů, když je větší než 80 % CPU průměrné hello napříč všemi instancemi.
* Vždycky dostat oznámení selhání e-mailem. Konkrétně hello vlastník, Přispěvatel a čtečky hello cílový prostředek dostávat e-maily. Také vždy zobrazí *obnovení* e-mail, když se obnoví v případě selhání škálování a začne fungovat normálně.
* Vám může výslovný souhlas tooreceive úspěšné škálování akci oznámení prostřednictvím e-mailu a pomocí webhooků.

## <a name="autoscale-best-practices"></a>Osvědčené postupy škálování
Použijte následující osvědčené postupy při používání automatického škálování hello.

### <a name="ensure-hello-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Ujistěte se, maximální a minimální hodnoty hello se liší a mít odpovídající okraje mezi nimi
Pokud máte nastavení, která má minimální = 2, maximální = 2 a aktuální počet instancí hello je 2, můžete dojít k žádné akci škálování. Zachovat odpovídající okraje mezi počty hello maximální a minimální instance, které jsou inkluzivní. Škálování vždy škáluje mezi tyto limity.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Ruční škálování při obnovení škálování min a max
Pokud ručně aktualizovat hello instance počet tooa hodnota výše nebo nižší než maximální hello hello škálování modul automaticky přizpůsobí back toohello minimální (Pokud se níže) nebo hello maximální (Pokud je vyšší než). Například můžete nastavit hello rozsahu 3 až 6. Pokud máte jeden spuštěnou instanci, modul škálování hello škáluje instance too3 při příštím spuštění. Podobně se by škálování in 8 instancí zpět too6 při příštím spuštění.  Ruční škálování je velmi dočasný, pokud resetujete hello škálování také pravidla.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Vždy používejte kombinaci Škálováním na více systémů a škálování pravidla, která provádí zvýšení a snížení
Pokud používáte pouze jednou ze součástí sady hello kombinaci, automatické škálování škálování in, který jednotné nebo ve, dokud neodstraníte hello maximální nebo minimální, je dosaženo.

### <a name="do-not-switch-between-hello-azure-portal-and-hello-azure-classic-portal-when-managing-autoscale"></a>Při správě škálování není přepínat mezi hello portál Azure a hello portál Azure classic
Pro cloudové služby a aplikace služeb (webové aplikace) použijte portál Azure (portal.azure.com) toocreate hello a spravovat nastavení automatického škálování. Pro sady škálování virtuálního počítače pomocí prostředí PowerShell, rozhraní příkazového řádku nebo REST API toocreate a spravovat nastavení automatického škálování. Není přepínat mezi hello portál Azure classic (adresu manage.windowsazure.com) a hello portálu Azure (portal.azure.com) při správě konfigurace automatického škálování. Hello portál Azure classic a jeho základní back-end má omezení. Přesuňte škálování Azure portálu toomanage toohello pomocí grafického uživatelského rozhraní. Možnosti Hello jsou toouse hello škálování prostředí PowerShell, rozhraní příkazového řádku nebo REST API (pomocí Průzkumníka prostředků Azure).

### <a name="choose-hello-appropriate-statistic-for-your-diagnostics-metric"></a>Zvolte hello příslušné statistiky pro vaše metrika diagnostiky
Diagnostika metriky, můžete vybrat mezi *průměrná*, *minimální*, *maximální* a *celkový* jako metriky tooscale podle. Nejběžnější Statistika Hello je *průměrná*.

### <a name="choose-hello-thresholds-carefully-for-all-metric-types"></a>Zvolte prahové hodnoty hello pečlivě pro všechny typy metriky
Doporučujeme pečlivě výběr různé prahové hodnoty pro Škálováním na více systémů a škálování in podle praktické situacích.

Jsme *nedoporučujeme* nastavení automatického škálování, jako příklady hello níže s hello stejné nebo velmi podobné prahové hodnoty pro odhlásit a v podmínkách:

* Zvýšit instancí 1 počet při počtu vláken < = 600
* Snižte instancí 1 počet při počtu vláken > = 600

Podívejme se na příklad co může způsobit tooa chování, které se může zdát matoucí. Vezměte v úvahu následující pořadí hello.

1. Předpokládejme, existují 2 instance toobegin s a pak hello průměrný počet vláken na instanci zvětšování too625.
2. Škálování horizontálně navýší kapacitu, přidání 3. instance.
3. V dalším kroku předpokládá, že počet hello průměrná vláken v instanci spadá too575.
4. Před škálování směrem dolů, automatické škálování pokusí tooestimate, jaké hello konečného stavu bude-li škálovat v. Například 575 x 3 (aktuální počet instancí) = 1,725 nebo 2 (konečný počet instancí při zmenšování) = 862.5 vláken. To znamená, že škálování by měla tooimmediately škálování znovu i po jeho škálovat, pokud hello vlákno průměrný počet zůstane hello stejný nebo i spadá pouze malé množství. Pokud ji škálovat znovu hello celý proces by opakovat, ale úvodní tooan nekonečná smyčka.
5. tooavoid této situaci (dále jen "flapping"), škálování není snižovat vůbec. Místo toho přeskočí a reevaluates hello podmínku znovu spustí úlohy hello další čas hello služby. To může matou Spousta lidí, protože škálování by se toowork, když byl 575 hello vlákno průměrný počet.

Odhad během škálování v je určený tooavoid "netřepotá" situace, kdy akce škálování in a škálování průběžně přejděte a zpět. Toto chování mít na paměti, když zvolíte hello stejné prahové hodnoty pro Škálováním na více systémů a na.

Doporučujeme vybrat odpovídající okraje mezi hello Škálováním na více systémů a prahové hodnoty. Jako příklad zvažte následující kombinace lepší pravidlo hello.

* Zvýšit instancí 1 počet při využití procesoru % > = 80
* Snižte instancí 1 počet při % využití procesoru < = 60

V takovém případě  

1. Předpokládejme, že se 2 instance toostart s.
2. Pokud hello průměrné využití procesoru % napříč instancemi přestane too80, automatické škálování horizontálně navýší kapacitu, přidání třetí instance.
3. Nyní předpokládá, že přes čas procesoru hello spadá % too60.
4. Škálování na škálování v pravidlo odhadne hello konečného stavu by měla tooscale v. Například 60 x 3 (aktuální počet instancí) = 180 nebo 2 (konečný počet instancí při zmenšování) = 90. Proto škálování není škálování – v protože by mohlo mít ihned tooscale na více systémů. Místo toho přeskočí škálování směrem dolů.
5. kontroluje, Hello další čas škálování hello procesoru pokračuje toofall too50. Odhadne znovu - instance 50 x 3 = 150 / 2 instance = 75, což je pod prahovou hodnotou škálování hello 80, takže ho škáluje úspěšně too2 instancí.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Důležité informace pro škálování prahové hodnoty pro speciální metriky
 Speciální metrik, jako je délka metrika úložiště nebo frontou Service Bus je prahová hodnota hello hello průměrný počet zpráv, které jsou k dispozici na aktuální počet instancí. Pečlivě zvolte hello zvolte hello prahová hodnota pro tuto metriku.

Umožňuje znázornění ho s tooensure příklad porozumíte hello chování lepší.

* Zvýšit instancí podle počtu 1, když počet zprávy fronty úložiště > = 50
* Snižte instancí 1 počet při fronty úložiště zpráv počet < = 10

Vezměte v úvahu následující pořadí hello:

1. Existují 2 instance fronty úložiště.
2. Zachovat zprávy přicházející a při kontrole fronty úložiště hello celkový počet hello přečte 50. Může předpokládat, že tento škálování by se měl spustit akci škálování. Ale Všimněte si, že je stále 50/2 = 25 zprávy na jednu instanci. Škálováním na více systémů, takže nedojde. Pro škálování toohappen první hello hello celkový počet zpráv ve frontě hello úložiště by měl být 100.
3. V dalším kroku předpokládá, že hello celkový počet zpráv dosáhne 100.
4. 3. instance fronty úložiště je přidat z důvodu akce tooa Škálováním na více systémů.  Hello další akce Škálováním na více systémů se neprovede dokud hello celkový počet zpráv ve frontě hello dosáhne 150, protože 150/3 = 50.
5. Nyní hello počet zpráv ve frontě hello získá menší. 3 instancemi hello první škálování v akce se stane, když hello celkový počet zpráv ve všech frontách dohromady too30 protože 30/3 = 10 zprávy každou instanci, která je hello škálování v prahovou hodnotu.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Důležité informace pro škálování při více profilů konfigurované v nastavení automatického škálování
V nastavení automatického škálování, můžete výchozí profil, který se použije vždy bez závislost na plán nebo čas, nebo můžete opakovaně profil nebo po stanovenou dobu s rozsahem datum a čas.

Při škálování služby je zpracuje, vždy kontroluje v hello následující pořadí:

1. Opravené datum profilu
2. Opakované profilu
3. Výchozí profil ("vždy")

Pokud je splněna podmínka profilu, nekontroluje škálování hello další podmínky profil pod ním. Škálování zpracovává jenom jeden profil najednou. To znamená, pokud chcete, aby tooalso zahrnují podmínku zpracování z profilu nižší úrovně, tato pravidla musí obsahovat také v aktuální profil hello.

Pojďme si to pomocí příklad:

Následující obrázek Hello ukazuje na nastavení automatického škálování s minimální instancí výchozí profil = 2 a maximální instancí = 10. V tomto příkladu jsou pravidla nakonfigurované tooscale na více systémů, v případě hello počet zpráv ve frontě hello je větší než 10 a škálování hello počet zpráv ve frontě hello je menší než 3. Hello prostředků, takže teď můžete škálovat mezi instancemi 2 až 10.

Kromě toho je profil opakovaně nastavit pro pondělí. Nastavení pro minimální instance = 2 a maximální instancí = 12. To znamená v pondělí, zkontroluje hello první čas škálování pro tuto podmínku, pokud je počet instancí hello 2, se škáluje toohello nové minimálně 3. Tak dlouho, dokud škálování pokračuje toofind tuto podmínku profil shodná (pondělí), zpracovává jenom hello bázi procesoru Škálováním na více systémů nebo v nakonfigurovaná pravidla pro tento profil. V tomto okamžiku nekontroluje se pro délka fronty hello. Ale pokud také chcete hello fronty délka podmínky toobe zaškrtnuto, by měla obsahovat tato pravidla z hello výchozí profil také váš profil pondělí.

Podobně když škálování přepne back toohello výchozí profil, nejdřív zkontroluje, pokud jsou splněny podmínky minimální a maximální hello. Pokud hello počet instancí v době hello 12, se škáluje v too10, hello maximální povolené pro hello výchozí profil.

![nastavení automatického škálování](./media/insights-autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Důležité informace pro škálování při nastavení víc pravidel v profilu
Existují případy, kdy můžete mít tooset více pravidel v profilu. služby používají používá Hello následující sadu pravidel škálování více pravidel jsou nastavena.

Na *škálovat*, škálování spustí, pokud je splněna žádné pravidlo.
Na *škálování v*, automatické škálování vyžadují toobe splněná všechna pravidla.

tooillustrate, předpokládají, že byl hello následující 4 škálování pravidla:

* Pokud procesoru < 30 % škálování v 1
* Pokud škálování v 1 paměti < 50 %
* Pokud > 75 % využití procesoru, horizontální o 1
* Pokud paměti > 75 %, horizontální o 1

Potom se provede hello postupujte podle kroků:

* Pokud je 76 % procesoru a paměti je 50 %, jsme Škálováním na více systémů.
* Pokud je 50 % využití procesoru a paměti je % 76 jsme Škálováním na více systémů.

Na druhé straně, hello, pokud využití procesoru činí 25 % a paměti je 51 % automatického škálování nemá **není** škálování v. V pořadí tooscale v procesoru musí být 29 % a paměť 49 %.

### <a name="always-select-a-safe-default-instance-count"></a>Vždy vybrat bezpečné výchozí počet instancí
Výchozí počet instancí Hello je důležité automatické škálování škáluje spočítat toothat služby metriky nejsou k dispozici. Proto vyberte výchozí počet instancí, který je bezpečný pro úlohy.

### <a name="configure-autoscale-notifications"></a>Konfigurace automatického škálování oznámení
Škálování oznámí správci hello a přispěvatelé hello prostředku e-mailem, pokud dojde k některé z hello následující podmínky:

* škálování služby nezdaří tootake akce.
* Metriky nejsou k dispozici pro škálování služby toomake škálování rozhodnutí.
* Metriky jsou k dispozici (obnovení) znovu toomake škálování rozhodnutí.
  Kromě výše uvedené podmínky toohello, můžete nakonfigurovat e-mailu nebo webhooku tooget oznámení, oznámení pro akce úspěšné škálování.
  
Můžete použít také protokolu aktivit výstrahy toomonitor hello stavu hello škálování stroje. Zde jsou příklady příliš[vytvořit aktivity protokolu výstrahy toomonitor všechny operace škálování modul vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) nebo příliš[vytvořit aktivity protokolu výstrahy toomonitor všechny nemohl škálování měřítka ve / škálovat operací na Vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

## <a name="next-steps"></a>Další kroky
- [Vytvoření aktivity protokolu výstrahy toomonitor všechny operace škálování modul vaše předplatné.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Vytvoření aktivity protokolu výstrahy toomonitor všechny nemohl škálování měřítka ve / škálovat operací na vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)
