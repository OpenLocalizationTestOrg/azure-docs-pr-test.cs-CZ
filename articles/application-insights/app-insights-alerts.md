---
title: "Výstrahy aaaSet ve službě Azure Application Insights | Microsoft Docs"
description: "Upozorňování o pomalé odezvy, výjimky a další výkonu nebo použití změn ve vaší webové aplikaci."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: e160cb173e68fda2e6d97f29da342c46b7ac4f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-alerts-in-application-insights"></a>Nastavit výstrahy ve službě Application Insights
[Azure Application Insights] [ start] vás může upozornit, toochanges v metriky výkonu a využití ve vaší webové aplikaci. 

Application Insights monitoruje vaše živé aplikace na [širokou škálu platformy] [ platforms] toohelp diagnostikovat problémy s výkonem a porozumět různým způsobům využití.

Existují tři typy výstrah:

* **Metriky výstrahy** zjistíte, kdy metriky protne prahovou hodnotu po nějakou dobu – například dobu odezvy, počty výjimky, využití procesoru nebo zobrazení stránky. 
* [**Webové testy** ] [ availability] zjistíte, pokud váš webový server je k dispozici na hello Internetu nebo reagovat pomalu. [Další informace][availability].
* [**Proaktivní diagnostiky** ](app-insights-proactive-diagnostics.md) jsou konfigurovány automaticky toonotify ohledně vzory neobvyklou výkonu.

Zaměříme na metriky výstrahy v tomto článku.

## <a name="set-a-metric-alert"></a>Nastavení metriky výstrahy
Okno otevřít hello pravidla výstrah a potom použijte hello tlačítko Přidat. 

![V okně hello pravidla výstrah zvolte Přidat výstrahy. Nastavit aplikaci jako hello toomeasure prostředků, zadejte název pro hello výstrahu a vyberte metriky.](./media/app-insights-alerts/01-set-metric.png)

* Nastavte další vlastnosti prostředku hello před hello. **Vyberte prostředek, hello "(součásti)"** Pokud chcete generovat výstrahy tooset na metriky výkonu a využití.
* Název Hello poskytněte toohello výstrahy musí být jedinečný v rámci skupiny prostředků hello (ne jenom aplikace).
* Být opatrní toonote hello jednotky, ve kterých se dotaz tooenter hello prahovou hodnotu.
* Pokud zaškrtnete políčko hello "E-mailu vlastníky...", jsou odesílány výstrahy tooeveryone e-mailu, který má skupinu prostředků toothis přístup. je toto nastavení uživatelů, tooexpand přidejte toohello [skupiny prostředků nebo předplatného](app-insights-resources-roles-access-control.md) (není hello prostředků).
* Pokud zadáte "Další e-mailů", výstrahy se posílají toothose jednotlivcům nebo skupin (zda je zaškrtnuto políčko "e-mailu vlastníky..." hello). 
* Nastavte [webhooku adresu](../monitoring-and-diagnostics/insights-webhooks-alerts.md) Pokud jste vytvořili webovou aplikaci, která odpovídá tooalerts. Je volána při aktivaci výstrahy hello i při vyřešení. (Ale Všimněte si, že se v současné době, parametry dotazu nejsou předána jako vlastnosti webhooku.)
* Můžete zakázat nebo povolit hello výstraha: najdete v části hello tlačítka v horní části hello hello okna.

*Výstrahy tlačítko Přidat hello se nezobrazí.* 

* Používáte účet organizace? Výstrahy můžete nastavit, pokud jste vlastníkem nebo přispěvatelem prostředek aplikace toothis přístup. Podívejte se na okno hello řízení přístupu. [Další informace o řízení přístupu][roles].

> [!NOTE]
> V okně výstrahy hello, zjistíte, že je již sadu výstrah: [proaktivní diagnostiky](app-insights-proactive-failure-diagnostics.md). Automatické výstraha Hello monitoruje jeden konkrétní metriky, žádost míra selhání. Pokud se rozhodnete toodisable hello proaktivní výstrahu, není nutné tooset vlastní upozornění na selhání rychlost požadavků. 
> 
> 

## <a name="see-your-alerts"></a>Zobrazit oznámení
Zobrazí se e-mailu, když do stavu výstrahy změny mezi neaktivní a aktivní. 

aktuální stav Hello Každá výstraha se zobrazí v okně hello pravidla výstrah.

Je souhrn poslední aktivitu na účtu ve výstrahách hello rozevíracího seznamu:

![Rozevírací seznam výstrah](./media/app-insights-alerts/010-alert-drop.png)

Historie Hello změny stavu je v hello protokol aktivit:

![V okně Přehled hello klikněte na tlačítko Nastavení, protokoly auditu](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Jak výstrahy fungují
* Výstraha má tři stavy: "Nikdy neaktivoval", "Aktivované" a "Přeložit". Aktivovaný znamená hello podmínku, kterou jste zadali, byla nastavena hodnota true, pokud bylo naposled vyhodnoceno.
* Oznámení se vygeneruje, když výstrahu změny stavu. (Pokud výstražný stav hello již byla hodnota true, když jste vytvořili výstrahu hello, nemusí získáte oznámení dokud hello podmínku přejde false.)
* Jednotlivá oznámení vygeneruje e-mailu v případě zaškrtnutí pole e-mailů hello nebo zadat e-mailové adresy. Můžete také prohlédnout hello oznámení rozevíracího seznamu.
* Výstraha je vyhodnocován pokaždé, když dorazí metriky, ale není jinak.
* vyhodnocení Hello agreguje hello metrika přes hello předcházející období a porovná je toohello prahová hodnota toodetermine hello nový stav.
* Hello období, který zvolíte, určuje hello interval, za které se agregovat metriky. Nemá vliv, hello výstraha četnost: to závisí na hello frekvenci příchodem metrik.
* Pokud žádná data doručení pro konkrétní metriku nějakou dobu, mezeru hello má jiný dopadů na výstrahy vyhodnocení a na hello grafy v Průzkumníku metriky. V Průzkumníku metriky Pokud je vidět žádná data po dobu delší než interval vzorkování hello grafu, hello graf znázorňuje s hodnotou 0. Ale výstrahu na základě hello stejné metriky není znovu vyhodnocena a hello výstrahy stavu nemění. 
  
    Pokud data nakonec dorazí, hello grafu přejde zpět tooa nenulovou hodnotu. vyhodnotí Hello výstrahy na základě dat hello k dispozici pro hello období, které jste zadali. Nový datový bod hello hello pouze jeden, které jsou k dispozici v období hello se hello agregační je založen na právě na datových bodů.
* Výstrahu můžete často blikat mezi stavy výstrah a v pořádku, i když nastavíte dlouhou dobu. To může dojít, pokud hodnota metriky hello bude umístěn kolem hello prahovou hodnotu. Neexistuje žádné hystereze v hello prahová hodnota: tooalert přechod hello se odehrává na stejnou hodnotu jako hello přechod toohealthy hello.

## <a name="what-are-good-alerts-tooset"></a>Jaké jsou dobrou výstrahy tooset?
To závisí na vaší aplikace. toostart, ho má osvědčené není tooset příliš mnoho metriky. Věnujte nějaký čas prohlížení vaší metriky grafy, když aplikace běží, tooget chování pro jak se chová normálně. Tento postup vám pomůže najít způsoby tooimprove jeho výkon. Pak nastavení výstrahy tootell jste při hello metriky výlet za hranice hello normální zóny. 

Oblíbené výstrahy patří:

* [Prohlížeč metriky][client], zejména prohlížeče **časů načtení stránky**, jsou vhodné pro webové aplikace. Pokud stránka má mnoho skriptů, je vhodné vyhledat **výjimky prohlížeče**. V pořadí tooget tyto metriky a výstrahy, máte tooset [monitorování webové stránky][client].
* **Doba odezvy serveru** pro webové aplikace na straně serveru hello. A také nastavení výstrah, dohlížet na tento metriky toosee Pokud se mění nepřiměřeně podle požadavků vysoké: variace, může znamenat, vaše aplikace může být nedostatek prostředků. 
* **Výjimky serveru** -toosee je, máte toodo některé [další nastavení](app-insights-asp-net-exceptions.md).

Nezapomeňte, že [proaktivní selhání míra diagnostiky](app-insights-proactive-failure-diagnostics.md) automaticky monitorování hello míra jakou aplikace reaguje toorequests s kódy selhání. 

## <a name="automation"></a>Automation
* [Pomocí prostředí PowerShell tooautomate nastavení výstrah](app-insights-powershell-alerts.md)
* [Pomocí webhooků tooautomate odpovídá tooalerts](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Viz také
* [Testy dostupnosti webu](app-insights-monitor-web-app-availability.md)
* [Automatizovat nastavení výstrah](app-insights-powershell-alerts.md)
* [Proaktivní diagnostiky](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

