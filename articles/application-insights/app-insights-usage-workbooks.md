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
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a><span data-ttu-id="cb322-103">Prozkoumat a sdílet data o využití s interaktivní sešity ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="cb322-103">Investigate and share usage data with interactive workbooks in Application Insights</span></span>

<span data-ttu-id="cb322-104">Sešity kombinovat [Azure Application Insights](app-insights-overview.md) vizualizaci dat, [analytické dotazy](app-insights-analytics.md)a text do interaktivní dokumenty.</span><span class="sxs-lookup"><span data-stu-id="cb322-104">Workbooks combine [Azure Application Insights](app-insights-overview.md) data visualizations, [Analytics queries](app-insights-analytics.md), and text into interactive documents.</span></span> <span data-ttu-id="cb322-105">Sešity jsou upravovat ostatní členové týmu s toohello přístup stejné prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="cb322-105">Workbooks are editable by other team members with access toohello same Azure resource.</span></span> <span data-ttu-id="cb322-106">To znamená, že hello dotazy a ovládací prvky používané toocreate sešitu jsou k dispozici tooother osoby, které čtení hello sešitu, a přitom snadno tooexplore, rozšířit a vyhledejte chyby.</span><span class="sxs-lookup"><span data-stu-id="cb322-106">This means hello queries and controls used toocreate a workbook are available tooother people reading hello workbook, making them easy tooexplore, extend, and check for mistakes.</span></span>

<span data-ttu-id="cb322-107">Sešity jsou užitečné pro scénáře, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="cb322-107">Workbooks are helpful for scenarios like:</span></span>

* <span data-ttu-id="cb322-108">Zkoumání hello využití aplikace, pokud neznáte hello metriky týkající se předem: počtu uživatelů, míru uchování, převod sazby atd. Na rozdíl od jiných nástrojů analýzy využití ve službě Application Insights sešity umožňují kombinovat více typů vizualizace a analýzy, přitom výborně hodí pro tento druh zkoumání volného tvaru.</span><span class="sxs-lookup"><span data-stu-id="cb322-108">Exploring hello usage of your app when you don't know hello metrics of interest in advance: numbers of users, retention rates, conversion rates, etc. Unlike other usage analytics tools in Application Insights, workbooks let you combine multiple kinds of visualizations and analyses, making them great for this kind of free-form exploration.</span></span>
* <span data-ttu-id="cb322-109">Vysvětlení tooyour týmu, jaký je výkon nově vydané funkce, uživatelem zobrazující počty pro klíče interakce a další metriky.</span><span class="sxs-lookup"><span data-stu-id="cb322-109">Explaining tooyour team how a newly released feature is performing, by showing user counts for key interactions and other metrics.</span></span>
* <span data-ttu-id="cb322-110">Sdílení hello výsledky A / B experimentovat ve vaší aplikaci s jinými členy týmu.</span><span class="sxs-lookup"><span data-stu-id="cb322-110">Sharing hello results of an A/B experiment in your app with other members of your team.</span></span> <span data-ttu-id="cb322-111">Mohou vysvětlovat hello cíle pro hello experimentovat s textem a pak zobrazit jednotlivé metriky využití a analýzy dotaz, použít tooevaluate hello experiment, společně s zrušte značek pro jestli jednotlivé metriky byla výše - nebo pod cíl.</span><span class="sxs-lookup"><span data-stu-id="cb322-111">You can explain hello goals for hello experiment with text, then show each usage metric and Analytics query used tooevaluate hello experiment, along with clear call-outs for whether each metric was above- or below-target.</span></span>
* <span data-ttu-id="cb322-112">Zprávy o využití hello vaší aplikace a kombinování dat, text vysvětlení a diskuzi o další kroky tooprevent výpadky hello budoucí dopad hello výpadku.</span><span class="sxs-lookup"><span data-stu-id="cb322-112">Reporting hello impact of an outage on hello usage of your app, combining data, text explanation, and a discussion of next steps tooprevent outages in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="cb322-113">Prostředku Application Insights musí obsahovat zobrazení stránek nebo sešity toouse vlastních událostí.</span><span class="sxs-lookup"><span data-stu-id="cb322-113">Your Application Insights resource must contain page views or custom events toouse workbooks.</span></span> <span data-ttu-id="cb322-114">[Zjistěte, jak tooset vaší aplikace toocollect stránku zobrazení automaticky s hello Application Insights JavaScript SDK](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="cb322-114">[Learn how tooset up your app toocollect page views automatically with hello Application Insights JavaScript SDK](app-insights-javascript.md).</span></span>
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a><span data-ttu-id="cb322-115">Úpravy, změna uspořádání, klonování a odstranění oddílů sešitu</span><span class="sxs-lookup"><span data-stu-id="cb322-115">Editing, rearranging, cloning, and deleting workbook sections</span></span>

<span data-ttu-id="cb322-116">Sešit je učiněna oddílů, které jsou: nezávisle upravitelné využití vizualizace, grafy, tabulky, text nebo analýza výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb322-116">A workbook is a made of sections: independently editable usage visualizations, charts, tables, text, or Analytics query results.</span></span>

<span data-ttu-id="cb322-117">obsah hello tooedit sešitu oddíl, klikněte na tlačítko hello **upravit** níže uvedené tlačítko a toohello pravé části hello sešitu.</span><span class="sxs-lookup"><span data-stu-id="cb322-117">tooedit hello contents of a workbook section, click hello **Edit** button below and toohello right of hello workbook section.</span></span>

![Application Insights sešity část ovládací prvky pro úpravy](./media/app-insights-usage-workbooks/editing-controls.png)

1. <span data-ttu-id="cb322-119">Po dokončení úprav oddíl, klikněte na tlačítko **udělat úpravy** v hello levém dolním rohu hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="cb322-119">When you're done editing a section, click **Done Editing** in hello bottom left corner of hello section.</span></span>

2. <span data-ttu-id="cb322-120">toocreate duplicitní oddíl, klikněte na tlačítko hello **klonovat v této části** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cb322-120">toocreate a duplicate of a section, click hello **Clone this section** icon.</span></span> <span data-ttu-id="cb322-121">Vytvoření duplicitní části je skvělým tooway tooiterate na dotazu bez ztráty předchozí iterací.</span><span class="sxs-lookup"><span data-stu-id="cb322-121">Creating duplicate sections is a great tooway tooiterate on a query without losing previous iterations.</span></span>

3. <span data-ttu-id="cb322-122">toomove si oddíl v sešitu, klikněte na tlačítko hello **přesunout nahoru** nebo **přesunout dolů** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cb322-122">toomove up a section in a workbook, click hello **Move up** or **Move down** icon.</span></span>

4. <span data-ttu-id="cb322-123">tooremove oddíl trvale, klikněte na hello **odebrat** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cb322-123">tooremove a section permanently, click hello **Remove** icon.</span></span>

## <a name="adding-usage-data-visualization-sections"></a><span data-ttu-id="cb322-124">Přidání oddílů vizualizace dat využití</span><span class="sxs-lookup"><span data-stu-id="cb322-124">Adding usage data visualization sections</span></span>

<span data-ttu-id="cb322-125">Sešity nabízí čtyři typy vizualizací analytics předdefinované využití.</span><span class="sxs-lookup"><span data-stu-id="cb322-125">Workbooks offer four types of built-in usage analytics visualizations.</span></span> <span data-ttu-id="cb322-126">Všechny odpovědi na běžné otázky o hello využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb322-126">Each answers a common question about hello usage of your app.</span></span> <span data-ttu-id="cb322-127">tooadd tabulky a grafy než tyto čtyři oddíly, přidejte částech dotazu Analytics (viz níže).</span><span class="sxs-lookup"><span data-stu-id="cb322-127">tooadd tables and charts other than these four sections, add Analytics query sections (see below).</span></span>

<span data-ttu-id="cb322-128">tooadd uživatelů, relací, události nebo uchování části tooyour sešitu, použijte hello **přidat uživatele** nebo jiné odpovídající tlačítko dolnímu hello hello sešitu nebo v dolní části hello všechny části.</span><span class="sxs-lookup"><span data-stu-id="cb322-128">tooadd a Users, Sessions, Events, or Retention section tooyour workbook, use hello **Add Users** or other corresponding button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

![Část uživatelé v sešitech](./media/app-insights-usage-workbooks/users-section.png)

<span data-ttu-id="cb322-130">**Uživatelé** části zodpovědět "počet uživatelů, kteří zobrazit některé stránky nebo použít některé funkce mé lokality?"</span><span class="sxs-lookup"><span data-stu-id="cb322-130">**Users** sections answer "How many users viewed some page or used some feature of my site?"</span></span>

<span data-ttu-id="cb322-131">**Relace** části zodpovědět "kolik relace jinak uživatelé vynaložili na zobrazení některých stránky nebo pomocí některé funkce mé lokality?"</span><span class="sxs-lookup"><span data-stu-id="cb322-131">**Sessions** sections answer "How many sessions did users spend viewing some page or using some feature of my site?"</span></span>

<span data-ttu-id="cb322-132">**Události** části zodpovědět "kolikrát uživatelé zobrazit některé stránku nebo pomocí některé funkce mé lokality?"</span><span class="sxs-lookup"><span data-stu-id="cb322-132">**Events** sections answer "How many times did users view some page or use some feature of my site?"</span></span>

<span data-ttu-id="cb322-133">Každý z těchto typů tři části nabízí stejné sady ovládacích prvků a vizualizací hello:</span><span class="sxs-lookup"><span data-stu-id="cb322-133">Each of these three section types offers hello same sets of controls and visualizations:</span></span>

* [<span data-ttu-id="cb322-134">Další informace o úpravách části uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="cb322-134">Learn more about editing Users, Sessions, and Events sections</span></span>](app-insights-usage-segmentation.md)
* <span data-ttu-id="cb322-135">Přepněte hello hlavní graf, histogram mřížky, automatické přehledy a vizualizací uživatelé ukázka pomocí hello **zobrazit graf**, **zobrazit mřížky**, **zobrazit Statistika**a **Ukázkové tito uživatelé** zaškrtávací políčka v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="cb322-135">Toggle hello main chart, histogram grids, automatic insights, and sample users visualizations using hello **Show Chart**, **Show Grid**, **Show Insights**, and **Sample of These Users** checkboxes at hello top of each section.</span></span>

![Uchování části v sešitech](./media/app-insights-usage-workbooks/retention-section.png)

<span data-ttu-id="cb322-137">**Uchování** části zodpovědět "Lidí, kteří zobrazit některé stránky nebo použití některých funkcí na jeden den nebo týden, kolik se vrátila zpět v následných dne nebo týdne?"</span><span class="sxs-lookup"><span data-stu-id="cb322-137">**Retention** sections answer "Of people who viewed some page or used some feature on one day or week, how many came back in a subsequent day or week?"</span></span>

* [<span data-ttu-id="cb322-138">Další informace o úpravách části uchování</span><span class="sxs-lookup"><span data-stu-id="cb322-138">Learn more about editing Retention sections</span></span>](app-insights-usage-retention.md)
* <span data-ttu-id="cb322-139">Přepnutí hello volitelné celkové uchování grafu pomocí hello **zobrazit celkový uchování grafu** zaškrtávacího políčka v horní části hello hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="cb322-139">Toggle hello optional Overall Retention chart using hello **Show overall retention chart** checkbox at hello top of hello section.</span></span>

## <a name="adding-application-insights-analytics-sections"></a><span data-ttu-id="cb322-140">Přidání Application Insights Analytics oddílů</span><span class="sxs-lookup"><span data-stu-id="cb322-140">Adding Application Insights Analytics sections</span></span>

![Část analýzy v sešitech](./media/app-insights-usage-workbooks/analytics-section.png)

<span data-ttu-id="cb322-142">tooadd tooyour sešit část dotazu aplikace analýza statistiky aplikace použijte hello **přidat Analytics dotazu** tlačítko dolnímu hello hello sešitu nebo v dolní části hello všechny části.</span><span class="sxs-lookup"><span data-stu-id="cb322-142">tooadd an Application Insights Analytics query section tooyour workbook, use hello **Add Analytics query** button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

<span data-ttu-id="cb322-143">Oddílů dotazu Analytics můžete přidat libovolný dotazy přes data Application Insights do sešitů.</span><span class="sxs-lookup"><span data-stu-id="cb322-143">Analytics query sections let you add arbitrary queries over your Application Insights data into workbooks.</span></span> <span data-ttu-id="cb322-144">Tato možnost znamená, že části Analytics dotazu by měla být vaše přejděte toofor odpovědi na všechny otázky o vašem webu než hello čtyři uvedené výše pro uživatele, relace, události a uchovávání dat, jako je:</span><span class="sxs-lookup"><span data-stu-id="cb322-144">This flexibility means Analytics query sections should be your go-toofor answering any questions about your site other than hello four listed above for Users, Sessions, Events, and Retention, like:</span></span>

* <span data-ttu-id="cb322-145">Kolik výjimky nebyla vaší lokality throw během hello stejné časové období jako odmítnout využití?</span><span class="sxs-lookup"><span data-stu-id="cb322-145">How many exceptions did your site throw during hello same time period as a decline in usage?</span></span>
* <span data-ttu-id="cb322-146">Jaký byl distribuční hello časů načtení stránky pro zobrazení některých stránky uživatele?</span><span class="sxs-lookup"><span data-stu-id="cb322-146">What was hello distribution of page load times for users viewing some page?</span></span>
* <span data-ttu-id="cb322-147">Počet uživatelů, kteří zobrazit některé sada stránek ve vaší lokalitě, ale není některých dalších nastavení stránek?</span><span class="sxs-lookup"><span data-stu-id="cb322-147">How many users viewed some set of pages on your site, but not some other set of pages?</span></span> <span data-ttu-id="cb322-148">Pokud máte clustery uživatelů, kteří používají různé podmnožiny váš web funkci to může být užitečné toounderstand (použijte hello `join` operátor s hello `kind=leftanti` modifikátor v hello dotazovacího jazyka pro analýzy protokolů).</span><span class="sxs-lookup"><span data-stu-id="cb322-148">This can be useful toounderstand if you have clusters of users who use different subsets of your site's functionality (use hello `join` operator with hello `kind=leftanti` modifier in hello Log Analytics query language).</span></span>

<span data-ttu-id="cb322-149">Použití hello [analýzy protokolů dotazu referenční informace k jazyku](https://docs.loganalytics.io/) toolearn více informací o zápis dotazů.</span><span class="sxs-lookup"><span data-stu-id="cb322-149">Use hello [Log Analytics query language reference](https://docs.loganalytics.io/) toolearn more about writing queries.</span></span>

## <a name="adding-text-and-markdown-sections"></a><span data-ttu-id="cb322-150">Přidávání textu a části Markdownu</span><span class="sxs-lookup"><span data-stu-id="cb322-150">Adding text and Markdown sections</span></span>

<span data-ttu-id="cb322-151">Přidávání záhlaví, vysvětlení a komentáře tooyour sešity pomáhá zapnout sadu tabulky a grafy do komentáře.</span><span class="sxs-lookup"><span data-stu-id="cb322-151">Adding headings, explanations, and commentary tooyour workbooks helps turn a set of tables and charts into a narrative.</span></span> <span data-ttu-id="cb322-152">Části textu v sešitech podporují hello [syntaxe Markdownu](https://daringfireball.net/projects/markdown/) pro formátování textu, jako je záhlaví, tučné, kurzíva a seznamy s odrážkami.</span><span class="sxs-lookup"><span data-stu-id="cb322-152">Text sections in workbooks support hello [Markdown syntax](https://daringfireball.net/projects/markdown/) for text formatting, like headings, bold, italics, and bulleted lists.</span></span>

<span data-ttu-id="cb322-153">tooadd sešitu tooyour část textu, použijte hello **přidat text** tlačítko dolnímu hello hello sešitu nebo v dolní části hello všechny části.</span><span class="sxs-lookup"><span data-stu-id="cb322-153">tooadd a text section tooyour workbook, use hello **Add text** button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

## <a name="saving-and-sharing-workbooks-with-your-team"></a><span data-ttu-id="cb322-154">Ukládání a sdílení sešitů se svým týmem</span><span class="sxs-lookup"><span data-stu-id="cb322-154">Saving and sharing workbooks with your team</span></span>

<span data-ttu-id="cb322-155">Sešity jsou uloženy v rámci prostředek Application Insights, buď v hello **Moje sestavy** oddíl, který je privátní tooyou nebo v hello **sdílené sestavy** oddíl, který je přístupný tooeveryone s přístupem toohello prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="cb322-155">Workbooks are saved within an Application Insights resource, either in hello **My Reports** section that's private tooyou or in hello **Shared Reports** section that's accessible tooeveryone with access toohello Application Insights resource.</span></span> <span data-ttu-id="cb322-156">tooview všechny hello sešitů v hello prostředků, klikněte na tlačítko hello **otevřete** tlačítka na panelu akcí hello.</span><span class="sxs-lookup"><span data-stu-id="cb322-156">tooview all hello workbooks in hello resource, click hello **Open** button in hello action bar.</span></span>

<span data-ttu-id="cb322-157">tooshare sešit, který je aktuálně v **Moje sestavy**:</span><span class="sxs-lookup"><span data-stu-id="cb322-157">tooshare a workbook that's currently in **My Reports**:</span></span>

1. <span data-ttu-id="cb322-158">Klikněte na tlačítko **otevřete** panelu Akce hello</span><span class="sxs-lookup"><span data-stu-id="cb322-158">Click **Open** in hello action bar</span></span>
2. <span data-ttu-id="cb322-159">Klikněte na tlačítko "..." hello vedle hello sešitu chcete tooshare</span><span class="sxs-lookup"><span data-stu-id="cb322-159">Click hello "..." button beside hello workbook you want tooshare</span></span>
3. <span data-ttu-id="cb322-160">Klikněte na tlačítko **přesunout sestavy tooShared**.</span><span class="sxs-lookup"><span data-stu-id="cb322-160">Click **Move tooShared Reports**.</span></span>

<span data-ttu-id="cb322-161">Klikněte na tlačítko tooshare sešitu s odkazem nebo prostřednictvím e-mailu, **sdílenou složku** panelu hello akce.</span><span class="sxs-lookup"><span data-stu-id="cb322-161">tooshare a workbook with a link or via email, click **Share** in hello action bar.</span></span> <span data-ttu-id="cb322-162">Uvědomte si, že příjemci hello odkazu potřebovat přístup k prostředku toothis hello Azure portálu tooview hello sešitu.</span><span class="sxs-lookup"><span data-stu-id="cb322-162">Keep in mind that recipients of hello link need access toothis resource in hello Azure portal tooview hello workbook.</span></span> <span data-ttu-id="cb322-163">úpravy toomake příjemce potřebovat aspoň oprávnění přispěvatele pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="cb322-163">toomake edits, recipients need at least Contributor permissions for hello resource.</span></span>

<span data-ttu-id="cb322-164">toopin tooan sešitu tooa odkaz řídicího panelu Azure:</span><span class="sxs-lookup"><span data-stu-id="cb322-164">toopin a link tooa workbook tooan Azure Dashboard:</span></span>

1. <span data-ttu-id="cb322-165">Klikněte na tlačítko **otevřete** panelu Akce hello</span><span class="sxs-lookup"><span data-stu-id="cb322-165">Click **Open** in hello action bar</span></span>
2. <span data-ttu-id="cb322-166">Klikněte na tlačítko "..." hello vedle hello sešitu chcete toopin</span><span class="sxs-lookup"><span data-stu-id="cb322-166">Click hello "..." button beside hello workbook you want toopin</span></span>
3. <span data-ttu-id="cb322-167">Klikněte na tlačítko **Pin toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="cb322-167">Click **Pin toodashboard**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb322-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb322-168">Next steps</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb322-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb322-169">Next steps</span></span>
- <span data-ttu-id="cb322-170">využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="cb322-170">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="cb322-171">Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="cb322-171">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="cb322-172">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="cb322-172">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="cb322-173">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="cb322-173">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="cb322-174">Uchování</span><span class="sxs-lookup"><span data-stu-id="cb322-174">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="cb322-175">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="cb322-175">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="cb322-176">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="cb322-176">Add user context</span></span>](app-insights-usage-send-user-context.md)
    
