---
title: "data o využití aaaInvestigate a sdílenou složku s interaktivní sešity ve službě Azure Application Insights | Microsoft docs"
description: "Demografické údaje analýzy uživatelů vaší webové aplikace."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: bdcebe0f97fdad0a0b301df5950dc09698f5a4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Prozkoumat a sdílet data o využití s interaktivní sešity ve službě Application Insights

Sešity kombinovat [Azure Application Insights](app-insights-overview.md) vizualizaci dat, [analytické dotazy](app-insights-analytics.md)a text do interaktivní dokumenty. Sešity jsou upravovat ostatní členové týmu s toohello přístup stejné prostředků Azure. To znamená, že hello dotazy a ovládací prvky používané toocreate sešitu jsou k dispozici tooother osoby, které čtení hello sešitu, a přitom snadno tooexplore, rozšířit a vyhledejte chyby.

Sešity jsou užitečné pro scénáře, jako jsou:

* Zkoumání hello využití aplikace, pokud neznáte hello metriky týkající se předem: počtu uživatelů, míru uchování, převod sazby atd. Na rozdíl od jiných nástrojů analýzy využití ve službě Application Insights sešity umožňují kombinovat více typů vizualizace a analýzy, přitom výborně hodí pro tento druh zkoumání volného tvaru.
* Vysvětlení tooyour týmu, jaký je výkon nově vydané funkce, uživatelem zobrazující počty pro klíče interakce a další metriky.
* Sdílení hello výsledky A / B experimentovat ve vaší aplikaci s jinými členy týmu. Mohou vysvětlovat hello cíle pro hello experimentovat s textem a pak zobrazit jednotlivé metriky využití a analýzy dotaz, použít tooevaluate hello experiment, společně s zrušte značek pro jestli jednotlivé metriky byla výše - nebo pod cíl.
* Zprávy o využití hello vaší aplikace a kombinování dat, text vysvětlení a diskuzi o další kroky tooprevent výpadky hello budoucí dopad hello výpadku.

> [!NOTE]
> Prostředku Application Insights musí obsahovat zobrazení stránek nebo sešity toouse vlastních událostí. [Zjistěte, jak tooset vaší aplikace toocollect stránku zobrazení automaticky s hello Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Úpravy, změna uspořádání, klonování a odstranění oddílů sešitu

Sešit je učiněna oddílů, které jsou: nezávisle upravitelné využití vizualizace, grafy, tabulky, text nebo analýza výsledků dotazu.

obsah hello tooedit sešitu oddíl, klikněte na tlačítko hello **upravit** níže uvedené tlačítko a toohello pravé části hello sešitu.

![Application Insights sešity část ovládací prvky pro úpravy](./media/app-insights-usage-workbooks/editing-controls.png)

1. Po dokončení úprav oddíl, klikněte na tlačítko **udělat úpravy** v hello levém dolním rohu hello oddílu.

2. toocreate duplicitní oddíl, klikněte na tlačítko hello **klonovat v této části** ikonu. Vytvoření duplicitní části je skvělým tooway tooiterate na dotazu bez ztráty předchozí iterací.

3. toomove si oddíl v sešitu, klikněte na tlačítko hello **přesunout nahoru** nebo **přesunout dolů** ikonu.

4. tooremove oddíl trvale, klikněte na hello **odebrat** ikonu.

## <a name="adding-usage-data-visualization-sections"></a>Přidání oddílů vizualizace dat využití

Sešity nabízí čtyři typy vizualizací analytics předdefinované využití. Všechny odpovědi na běžné otázky o hello využití vaší aplikace. tooadd tabulky a grafy než tyto čtyři oddíly, přidejte částech dotazu Analytics (viz níže).

tooadd uživatelů, relací, události nebo uchování části tooyour sešitu, použijte hello **přidat uživatele** nebo jiné odpovídající tlačítko dolnímu hello hello sešitu nebo v dolní části hello všechny části.

![Část uživatelé v sešitech](./media/app-insights-usage-workbooks/users-section.png)

**Uživatelé** části zodpovědět "počet uživatelů, kteří zobrazit některé stránky nebo použít některé funkce mé lokality?"

**Relace** části zodpovědět "kolik relace jinak uživatelé vynaložili na zobrazení některých stránky nebo pomocí některé funkce mé lokality?"

**Události** části zodpovědět "kolikrát uživatelé zobrazit některé stránku nebo pomocí některé funkce mé lokality?"

Každý z těchto typů tři části nabízí stejné sady ovládacích prvků a vizualizací hello:

* [Další informace o úpravách části uživatelů, relací a události](app-insights-usage-segmentation.md)
* Přepněte hello hlavní graf, histogram mřížky, automatické přehledy a vizualizací uživatelé ukázka pomocí hello **zobrazit graf**, **zobrazit mřížky**, **zobrazit Statistika**a **Ukázkové tito uživatelé** zaškrtávací políčka v horní části hello.

![Uchování části v sešitech](./media/app-insights-usage-workbooks/retention-section.png)

**Uchování** části zodpovědět "Lidí, kteří zobrazit některé stránky nebo použití některých funkcí na jeden den nebo týden, kolik se vrátila zpět v následných dne nebo týdne?"

* [Další informace o úpravách části uchování](app-insights-usage-retention.md)
* Přepnutí hello volitelné celkové uchování grafu pomocí hello **zobrazit celkový uchování grafu** zaškrtávacího políčka v horní části hello hello oddílu.

## <a name="adding-application-insights-analytics-sections"></a>Přidání Application Insights Analytics oddílů

![Část analýzy v sešitech](./media/app-insights-usage-workbooks/analytics-section.png)

tooadd tooyour sešit část dotazu aplikace analýza statistiky aplikace použijte hello **přidat Analytics dotazu** tlačítko dolnímu hello hello sešitu nebo v dolní části hello všechny části.

Oddílů dotazu Analytics můžete přidat libovolný dotazy přes data Application Insights do sešitů. Tato možnost znamená, že části Analytics dotazu by měla být vaše přejděte toofor odpovědi na všechny otázky o vašem webu než hello čtyři uvedené výše pro uživatele, relace, události a uchovávání dat, jako je:

* Kolik výjimky nebyla vaší lokality throw během hello stejné časové období jako odmítnout využití?
* Jaký byl distribuční hello časů načtení stránky pro zobrazení některých stránky uživatele?
* Počet uživatelů, kteří zobrazit některé sada stránek ve vaší lokalitě, ale není některých dalších nastavení stránek? Pokud máte clustery uživatelů, kteří používají různé podmnožiny váš web funkci to může být užitečné toounderstand (použijte hello `join` operátor s hello `kind=leftanti` modifikátor v hello dotazovacího jazyka pro analýzy protokolů).

Použití hello [analýzy protokolů dotazu referenční informace k jazyku](https://docs.loganalytics.io/) toolearn více informací o zápis dotazů.

## <a name="adding-text-and-markdown-sections"></a>Přidávání textu a části Markdownu

Přidávání záhlaví, vysvětlení a komentáře tooyour sešity pomáhá zapnout sadu tabulky a grafy do komentáře. Části textu v sešitech podporují hello [syntaxe Markdownu](https://daringfireball.net/projects/markdown/) pro formátování textu, jako je záhlaví, tučné, kurzíva a seznamy s odrážkami.

tooadd sešitu tooyour část textu, použijte hello **přidat text** tlačítko dolnímu hello hello sešitu nebo v dolní části hello všechny části.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Ukládání a sdílení sešitů se svým týmem

Sešity jsou uloženy v rámci prostředek Application Insights, buď v hello **Moje sestavy** oddíl, který je privátní tooyou nebo v hello **sdílené sestavy** oddíl, který je přístupný tooeveryone s přístupem toohello prostředek Application Insights. tooview všechny hello sešitů v hello prostředků, klikněte na tlačítko hello **otevřete** tlačítka na panelu akcí hello.

tooshare sešit, který je aktuálně v **Moje sestavy**:

1. Klikněte na tlačítko **otevřete** panelu Akce hello
2. Klikněte na tlačítko "..." hello vedle hello sešitu chcete tooshare
3. Klikněte na tlačítko **přesunout sestavy tooShared**.

Klikněte na tlačítko tooshare sešitu s odkazem nebo prostřednictvím e-mailu, **sdílenou složku** panelu hello akce. Uvědomte si, že příjemci hello odkazu potřebovat přístup k prostředku toothis hello Azure portálu tooview hello sešitu. úpravy toomake příjemce potřebovat aspoň oprávnění přispěvatele pro prostředek hello.

toopin tooan sešitu tooa odkaz řídicího panelu Azure:

1. Klikněte na tlačítko **otevřete** panelu Akce hello
2. Klikněte na tlačítko "..." hello vedle hello sešitu chcete toopin
3. Klikněte na tlačítko **Pin toodashboard**.

## <a name="next-steps"></a>Další kroky

## <a name="next-steps"></a>Další kroky
- využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.
    - [Uživatelé, relace, události](app-insights-usage-segmentation.md)
    - [Trychtýře](usage-funnels.md)
    - [Uchování](app-insights-usage-retention.md)
    - [Toky uživatele](app-insights-usage-flows.md)
    - [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)
    
