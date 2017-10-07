---
title: "vzory navigační aaaAnalyze uživatele s toky uživatele ve službě Azure Application Insights | Microsoft docs"
description: "Analýza, jak uživatelé přecházejí mezi hello stránky a funkce vaší webové aplikace."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d3f35dc78e9874e4b7974604bf91c40a5e5b78eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Analýza vzory navigační uživatele s toky uživatele ve službě Application Insights

![Nástroj Přehled toků uživatele aplikace](./media/app-insights-usage-flows/flows.png)

Nástroj uživatele toků Hello vizualizuje, jak uživatelé přecházejí mezi hello stránky a funkce vašeho webu. Služba je skvělá pro odpovědi na otázky jako:
* Jak opustit uživatelům a na stránce na váš web?
* Co uživatelé klikněte na stránku na webu?
* Kde jsou hello umístí, že uživatelé změn většinu z vaší lokality?
* Existují míst, kde uživatelé opakování opakovaně hello stejnou akci?

Hello uživatele toků nástroj spustí z úvodní stránky zobrazení nebo událostí, který určíte. Danou tato zobrazení stránky nebo vlastní události, tok uživatele zobrazí text hello, zobrazení stránek a vlastních událostí, které uživatelé odeslány okamžitě později během relace, dva kroky později, a tak dále. Řádky různých tloušťky zobrazit více než jednou. Každá cesta následovalo uživatele. Speciální uzly "Relace skončila" zobrazit, kolik uživatelů odeslaných žádné zobrazení stránek nebo uživatelské události po předchozím uzlu hello zvýraznění, kde uživatelé pravděpodobně zbývajících vaší lokality.



> [!NOTE]
> Prostředku Application Insights musí obsahovat zobrazení stránek nebo vlastních událostí toouse hello uživatele toků nástroj. [Zjistěte, jak tooset vaší aplikace toocollect stránku zobrazení automaticky s hello Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Začněte tím, že vyberete zobrazení úvodní stránky nebo vlastní události

![Zvolte událost počáteční toky uživatele](./media/app-insights-usage-flows/flows-initial-event.png)

tooget spuštění odpovědi na otázky nástrojem hello toků uživatele, vyberte jako výchozí bod pro vizualizaci hello hello zobrazení úvodní stránky nebo tooserve vlastní události:
1. Kliknutím na odkaz hello v hello "co uživatelé udělat po...?" Title, nebo klikněte na tlačítko Upravit hello. 
2. Vyberte z rozevíracího seznamu "Počáteční událost" hello zobrazení stránky nebo vlastní události.
3. Klikněte na tlačítko "Vytvoření grafu".

sloupci "Krok 1" Hello hello vizualizace se zobrazuje, co uživatelé se nejčastěji jenom po počáteční události hello seřazené horní dolů z častých tooleast většinu. Hello "Krok 2" a následné sloupce zobrazení, se kterými se uživatelé následně vytváření obrázku všech uživatelů způsoby hello přešli svého webu.

Ve výchozím nastavení nástroj uživatele toků hello náhodně ukázky hello pouze posledních 24 hodin, zobrazení stránek a vlastní události z vaší lokality. Můžete zvýšit hello časový rozsah a změnit hello vyrovnání výkonu a přesnost pro náhodné vzorkování v nabídce Upravit hello.

Pokud některé hello zobrazení stránek a vlastních událostí nejsou relevantní tooyou, klikněte na tlačítko hello "X" na uzlech hello chcete toohide. Po dokončení výběru hello uzly, které chcete toohide, klikněte na tlačítko "Vytvoření grafu" hello níže hello vizualizace. toosee všechny uzly hello skrytých, klikněte na tlačítko Upravit hello a pak podívejte se na části "Vyloučené události" hello.

Pokud zobrazení stránek nebo uživatelské události chybí které očekáváte toosee vizualizace hello:
* Zkontrolujte v nabídce Upravit hello části "Vyloučené události" hello.
* Použití ovládacího prvku "Úroveň podrobností" hello v hello upravit nabídky tooinclude méně časté události v hello vizualizace.
* Pokud zobrazení stránky hello nebo vlastní události, které odesílají zřídka uživatelů, zkuste se zvyšující hello časové rozmezí hello vizualizace v nabídce Upravit hello.
* Ujistěte se, zda text hello zobrazení stránky nebo vlastní události, které očekáváte, že je nastavení toobe shromažďují hello Application Insights SDK ve zdrojovém kódu hello vašeho webu. [Další informace o shromažďování vlastních událostí.](app-insights-api-custom-events-metrics.md)

Pokud chcete upravit toosee více kroků v hello vizualizace, použijte hello "Počet kroků" jezdec v hello nabídky.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Po přečtení informací na stránky nebo funkce, které uživatelé navštěvují a co kliknou?

![Použít toounderstand toků uživatele, kde uživatelé kliknou na](./media/app-insights-usage-flows/flows-one-step.png)

Pokud je vaše počáteční událost zobrazení stránky, první sloupec hello ("krok 1) hello vizualizace je rychlý způsob toounderstand, se kterými se uživatelé okamžitě po přečtení informací na stránku hello. Otevřete váš web v další toohello okno vizualizace toků uživatele. Porovnejte vaše očekávání jak uživatelé komunikovat s hello stránky toohello seznam událostí ve sloupci "Krok 1" hello. Element uživatelského rozhraní na stránce hello, které zdá se, že zanedbatelný tooyour týmu často, může být mezi hello nejpoužívanějším na stránku hello. Může být skvělý výchozí bod pro návrh vylepšení tooyour lokality.

Pokud je vaše počáteční událost vlastní události, hello první sloupec zobrazuje, co uživatelé se jenom po provedení této akce. Stejně jako u zobrazení stránky, zvažte, pokud hello zaznamenali, že chování uživatelů odpovídá cílů a očekávání vašeho týmu. Pokud vaše vybrané počáteční události je "Added položky tooShopping košíku", například, vyhledejte toosee Pokud "Přejděte tooCheckout" a "Dokončení nákupu" se zobrazí v hello vizualizace krátce po tomto datu. Pokud chování uživatelů se výrazně liší od vašim požadavkům, použijte toounderstand vizualizace hello jak uživatelé jsou získávání "zachytí" aplikace aktuální návrh vašeho webu.

## <a name="where-are-hello-places-that-users-churn-most-from-your-site"></a>Kde jsou hello umístí, že uživatelé změn většinu z vaší lokality?

Sledovat "Relace skončila" uzlů, které zobrazují vysokou up ve sloupci v hello vizualizace, zejména časná v toku. To znamená mnoho uživatelů pravděpodobně churned z vaší lokality po následující hello výše uvedená cesta stránek a interakce uživatelského rozhraní. Někdy se očekává – po dokončení nákupu na stránku elektronické obchodování například - ale obvykle změn je znaménkem návrhu problémy, nízký výkon nebo jiných problémů s váš web, který je možné zlepšit.

Mít na paměti, že "relace skončila" uzly jsou pouze podle telemetrická data shromažďovaná společností tento prostředek Application Insights. Pokud Application Insights neobdrží telemetrie pro určité uživatelské interakce, uživatelé může stále mít zpracoval s webem tyto způsoby po hello uživatele toků nástroj uvádí hello relace skončila.

## <a name="are-there-places-where-users-repeat-hello-same-action-over-and-over"></a>Existují míst, kde uživatelé opakování opakovaně hello stejnou akci?

Podívejte se na zobrazení stránky nebo vlastní událost, která se opakuje mnoha uživateli v dalších krocích hello vizualizace. To obvykle znamená, že uživatelé fungují opakovaných akcí na váš web. Pokud zjistíte opakování, vezměte v úvahu Změna návrhu hello vaší lokality nebo přidání nové funkce tooreduce opakování. Například přidání hromadné úpravy funkce, pokud zjistíte, uživatelé provádění opakovaných akcí na každý řádek table element.

## <a name="common-questions"></a>Nejčastější dotazy

### <a name="why-do-steps-appear-disconnected"></a>Proč kroky zobrazí odpojené?

![Uživatel toků se odpojené kroky](./media/app-insights-usage-flows/flows-disconnected.png)

Odpojení kroků (sloupce) v vizualizace toků uživatele znamená, že žádná z cest hello provedenou uživatelů mezi kroky hello byly dostatečně častý toobe zobrazí. tooshow méně časté události na hello vizualizace takže hello kroky se zobrazí připojené, upravte jezdec "Úroveň podrobností" hello v nabídce Upravit hello.

### <a name="does-hello-initial-event-represent-hello-first-time-hello-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Hello počáteční události představují hello první čas hello události se zobrazí v relaci, nebo kdykoli, který se zobrazuje v relaci?

Hello počáteční událostí na hello vizualizace pouze představuje hello poprvé, co uživatel odeslaných tímto zobrazení stránky nebo vlastní události během relace. Pokud uživatelé mohou odesílat události počáteční hello více než jednou. v relaci, pak sloupec "Krok 1" hello se zobrazí pouze chování uživatele po hello *první* instanci počáteční události, ne všechny instance.

## <a name="next-steps"></a>Další kroky

* [Přehled využití](app-insights-usage-overview.md)
* [Uživatelů, relací a události](app-insights-usage-segmentation.md)
* [Uchování](app-insights-usage-retention.md)
* [Přidání vlastních událostí tooyour aplikace](app-insights-api-custom-events-metrics.md)
