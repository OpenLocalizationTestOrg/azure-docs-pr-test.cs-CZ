---
title: "Prozkoumat a sdílet data o využití s interaktivní sešity ve službě Azure Application Insights | Microsoft docs"
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
ms.openlocfilehash: 75028b4fbda43d90f56690a33c7eb624fce049c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a><span data-ttu-id="e0a5d-103">Prozkoumat a sdílet data o využití s interaktivní sešity ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="e0a5d-103">Investigate and share usage data with interactive workbooks in Application Insights</span></span>

<span data-ttu-id="e0a5d-104">Sešity kombinovat [Azure Application Insights](app-insights-overview.md) vizualizaci dat, [analytické dotazy](app-insights-analytics.md)a text do interaktivní dokumenty.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-104">Workbooks combine [Azure Application Insights](app-insights-overview.md) data visualizations, [Analytics queries](app-insights-analytics.md), and text into interactive documents.</span></span> <span data-ttu-id="e0a5d-105">Sešity jsou upravovat ostatní členové týmu s přístupem ke stejné prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-105">Workbooks are editable by other team members with access to the same Azure resource.</span></span> <span data-ttu-id="e0a5d-106">To znamená, že jsou k dispozici jiným uživatelům čtení sešit, tím je snadno prozkoumat, rozšířit a zkontrolujte chyby dotazy a ovládacích prvků používaná k vytvoření sešitu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-106">This means the queries and controls used to create a workbook are available to other people reading the workbook, making them easy to explore, extend, and check for mistakes.</span></span>

<span data-ttu-id="e0a5d-107">Sešity jsou užitečné pro scénáře, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="e0a5d-107">Workbooks are helpful for scenarios like:</span></span>

* <span data-ttu-id="e0a5d-108">Zkoumání využití aplikace, pokud neznáte metriky týkající se předem: počtu uživatelů, míru uchování, převod sazby atd. Na rozdíl od jiných nástrojů analýzy využití ve službě Application Insights sešity umožňují kombinovat více typů vizualizace a analýzy, přitom výborně hodí pro tento druh zkoumání volného tvaru.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-108">Exploring the usage of your app when you don't know the metrics of interest in advance: numbers of users, retention rates, conversion rates, etc. Unlike other usage analytics tools in Application Insights, workbooks let you combine multiple kinds of visualizations and analyses, making them great for this kind of free-form exploration.</span></span>
* <span data-ttu-id="e0a5d-109">Vysvětlení si s týmem, jaký je výkon nově vydané funkce, uživatelem zobrazující počty pro klíče interakce a další metriky.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-109">Explaining to your team how a newly released feature is performing, by showing user counts for key interactions and other metrics.</span></span>
* <span data-ttu-id="e0a5d-110">Sdílení výsledky A / B experimentovat ve vaší aplikaci s jinými členy týmu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-110">Sharing the results of an A/B experiment in your app with other members of your team.</span></span> <span data-ttu-id="e0a5d-111">Můžete vysvětlit cílů experimentu s textem a pak zobrazit každý využití metriky a analýzy dotaz byl použit k vyhodnocení experiment, společně s zrušte značek pro jestli jednotlivé metriky byla výše - nebo pod cíl.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-111">You can explain the goals for the experiment with text, then show each usage metric and Analytics query used to evaluate the experiment, along with clear call-outs for whether each metric was above- or below-target.</span></span>
* <span data-ttu-id="e0a5d-112">Dopad výpadku sestavy o využití vaší aplikace a kombinování dat, text vysvětlení a diskuzi o další kroky, abyste zabránili výpadkům v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-112">Reporting the impact of an outage on the usage of your app, combining data, text explanation, and a discussion of next steps to prevent outages in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="e0a5d-113">Prostředku Application Insights musí obsahovat zobrazení stránek nebo vlastních událostí k používání sešitů.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-113">Your Application Insights resource must contain page views or custom events to use workbooks.</span></span> <span data-ttu-id="e0a5d-114">[Zjistěte, jak nastavit aplikaci shromažďovat zobrazení stránky automaticky pomocí Application Insights JavaScript SDK](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="e0a5d-114">[Learn how to set up your app to collect page views automatically with the Application Insights JavaScript SDK](app-insights-javascript.md).</span></span>
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a><span data-ttu-id="e0a5d-115">Úpravy, změna uspořádání, klonování a odstranění oddílů sešitu</span><span class="sxs-lookup"><span data-stu-id="e0a5d-115">Editing, rearranging, cloning, and deleting workbook sections</span></span>

<span data-ttu-id="e0a5d-116">Sešit je učiněna oddílů, které jsou: nezávisle upravitelné využití vizualizace, grafy, tabulky, text nebo analýza výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-116">A workbook is a made of sections: independently editable usage visualizations, charts, tables, text, or Analytics query results.</span></span>

<span data-ttu-id="e0a5d-117">Upravit obsah části sešitu, klikněte **upravit** tlačítko níže a pravé části sešitu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-117">To edit the contents of a workbook section, click the **Edit** button below and to the right of the workbook section.</span></span>

![Application Insights sešity část ovládací prvky pro úpravy](./media/app-insights-usage-workbooks/editing-controls.png)

1. <span data-ttu-id="e0a5d-119">Po dokončení úprav oddíl, klikněte na tlačítko **udělat úpravy** v levém dolním rohu části.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-119">When you're done editing a section, click **Done Editing** in the bottom left corner of the section.</span></span>

2. <span data-ttu-id="e0a5d-120">Chcete-li vytvořit duplicitní oddíl, klikněte na tlačítko **klonovat v této části** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-120">To create a duplicate of a section, click the **Clone this section** icon.</span></span> <span data-ttu-id="e0a5d-121">Vytvoření duplicitní části je skvělá pro způsob, jak iterovat dotazu bez ztráty předchozí iterací.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-121">Creating duplicate sections is a great to way to iterate on a query without losing previous iterations.</span></span>

3. <span data-ttu-id="e0a5d-122">Chcete-li přesunout nahoru oddíl v sešitu, klikněte na tlačítko **přesunout nahoru** nebo **přesunout dolů** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-122">To move up a section in a workbook, click the **Move up** or **Move down** icon.</span></span>

4. <span data-ttu-id="e0a5d-123">Pokud chcete trvale odebrat oddíl, klikněte **odebrat** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-123">To remove a section permanently, click the **Remove** icon.</span></span>

## <a name="adding-usage-data-visualization-sections"></a><span data-ttu-id="e0a5d-124">Přidání oddílů vizualizace dat využití</span><span class="sxs-lookup"><span data-stu-id="e0a5d-124">Adding usage data visualization sections</span></span>

<span data-ttu-id="e0a5d-125">Sešity nabízí čtyři typy vizualizací analytics předdefinované využití.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-125">Workbooks offer four types of built-in usage analytics visualizations.</span></span> <span data-ttu-id="e0a5d-126">Všechny odpovědi na běžné otázky o využití vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-126">Each answers a common question about the usage of your app.</span></span> <span data-ttu-id="e0a5d-127">Pokud chcete přidat, tabulky a grafy než tyto čtyři oddíly, přidejte částech dotazu Analytics (viz níže).</span><span class="sxs-lookup"><span data-stu-id="e0a5d-127">To add tables and charts other than these four sections, add Analytics query sections (see below).</span></span>

<span data-ttu-id="e0a5d-128">Přidání uživatelů, relací, události nebo uchování části k sešitu, použijte **přidat uživatele** nebo jiné odpovídající tlačítko v dolní části sešitu nebo v dolní části všechny části.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-128">To add a Users, Sessions, Events, or Retention section to your workbook, use the **Add Users** or other corresponding button at the bottom of the workbook, or at the bottom of any section.</span></span>

![Část uživatelé v sešitech](./media/app-insights-usage-workbooks/users-section.png)

<span data-ttu-id="e0a5d-130">**Uživatelé** části zodpovědět "počet uživatelů, kteří zobrazit některé stránky nebo použít některé funkce mé lokality?"</span><span class="sxs-lookup"><span data-stu-id="e0a5d-130">**Users** sections answer "How many users viewed some page or used some feature of my site?"</span></span>

<span data-ttu-id="e0a5d-131">**Relace** části zodpovědět "kolik relace jinak uživatelé vynaložili na zobrazení některých stránky nebo pomocí některé funkce mé lokality?"</span><span class="sxs-lookup"><span data-stu-id="e0a5d-131">**Sessions** sections answer "How many sessions did users spend viewing some page or using some feature of my site?"</span></span>

<span data-ttu-id="e0a5d-132">**Události** části zodpovědět "kolikrát uživatelé zobrazit některé stránku nebo pomocí některé funkce mé lokality?"</span><span class="sxs-lookup"><span data-stu-id="e0a5d-132">**Events** sections answer "How many times did users view some page or use some feature of my site?"</span></span>

<span data-ttu-id="e0a5d-133">Každý z těchto typů tři části nabízí stejné sady ovládací prvky a vizualizací:</span><span class="sxs-lookup"><span data-stu-id="e0a5d-133">Each of these three section types offers the same sets of controls and visualizations:</span></span>

* [<span data-ttu-id="e0a5d-134">Další informace o úpravách části uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="e0a5d-134">Learn more about editing Users, Sessions, and Events sections</span></span>](app-insights-usage-segmentation.md)
* <span data-ttu-id="e0a5d-135">Přepnutí hlavní graf, histogram mřížky, automatické přehledy a vizualizací ukázka uživatele pomocí **zobrazit graf**, **zobrazit mřížky**, **zobrazit Statistika**, a **ukázka tito uživatelé** zaškrtávací políčka v horní části každé části.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-135">Toggle the main chart, histogram grids, automatic insights, and sample users visualizations using the **Show Chart**, **Show Grid**, **Show Insights**, and **Sample of These Users** checkboxes at the top of each section.</span></span>

![Uchování části v sešitech](./media/app-insights-usage-workbooks/retention-section.png)

<span data-ttu-id="e0a5d-137">**Uchování** části zodpovědět "Lidí, kteří zobrazit některé stránky nebo použití některých funkcí na jeden den nebo týden, kolik se vrátila zpět v následných dne nebo týdne?"</span><span class="sxs-lookup"><span data-stu-id="e0a5d-137">**Retention** sections answer "Of people who viewed some page or used some feature on one day or week, how many came back in a subsequent day or week?"</span></span>

* [<span data-ttu-id="e0a5d-138">Další informace o úpravách části uchování</span><span class="sxs-lookup"><span data-stu-id="e0a5d-138">Learn more about editing Retention sections</span></span>](app-insights-usage-retention.md)
* <span data-ttu-id="e0a5d-139">Můžete přepínat pomocí graf volitelné celkové uchovávání **zobrazit celkový uchování grafu** zaškrtávacího políčka v horní části.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-139">Toggle the optional Overall Retention chart using the **Show overall retention chart** checkbox at the top of the section.</span></span>

## <a name="adding-application-insights-analytics-sections"></a><span data-ttu-id="e0a5d-140">Přidání Application Insights Analytics oddílů</span><span class="sxs-lookup"><span data-stu-id="e0a5d-140">Adding Application Insights Analytics sections</span></span>

![Část analýzy v sešitech](./media/app-insights-usage-workbooks/analytics-section.png)

<span data-ttu-id="e0a5d-142">Přidat oddíl dotazu Application Insights Analytics k sešitu, použijte **přidat Analytics dotazu** tlačítko v dolní části sešitu nebo v dolní části všechny části.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-142">To add an Application Insights Analytics query section to your workbook, use the **Add Analytics query** button at the bottom of the workbook, or at the bottom of any section.</span></span>

<span data-ttu-id="e0a5d-143">Oddílů dotazu Analytics můžete přidat libovolný dotazy přes data Application Insights do sešitů.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-143">Analytics query sections let you add arbitrary queries over your Application Insights data into workbooks.</span></span> <span data-ttu-id="e0a5d-144">Tato možnost znamená, že Analytics částech dotazu by měla být vaše přejděte – chcete pro odpovědi na všechny otázky o vašem webu než čtyři uvedené výše pro uživatele, relace, události a uchovávání dat, jako je:</span><span class="sxs-lookup"><span data-stu-id="e0a5d-144">This flexibility means Analytics query sections should be your go-to for answering any questions about your site other than the four listed above for Users, Sessions, Events, and Retention, like:</span></span>

* <span data-ttu-id="e0a5d-145">Kolik výjimky váš web výjimku během časového období stejné jako odmítnout využití?</span><span class="sxs-lookup"><span data-stu-id="e0a5d-145">How many exceptions did your site throw during the same time period as a decline in usage?</span></span>
* <span data-ttu-id="e0a5d-146">Co se distribuce časů načtení stránky pro zobrazení některých stránky uživatele?</span><span class="sxs-lookup"><span data-stu-id="e0a5d-146">What was the distribution of page load times for users viewing some page?</span></span>
* <span data-ttu-id="e0a5d-147">Počet uživatelů, kteří zobrazit některé sada stránek ve vaší lokalitě, ale není některých dalších nastavení stránek?</span><span class="sxs-lookup"><span data-stu-id="e0a5d-147">How many users viewed some set of pages on your site, but not some other set of pages?</span></span> <span data-ttu-id="e0a5d-148">To může být užitečné zjistit, jestli máte clustery uživatelů, kteří používají různé podmnožiny váš web funkci (pomocí `join` operátor s `kind=leftanti` modifikátor v jazyce dotazu analýzy protokolů).</span><span class="sxs-lookup"><span data-stu-id="e0a5d-148">This can be useful to understand if you have clusters of users who use different subsets of your site's functionality (use the `join` operator with the `kind=leftanti` modifier in the Log Analytics query language).</span></span>

<span data-ttu-id="e0a5d-149">Použití [analýzy protokolů dotazu referenční informace k jazyku](https://docs.loganalytics.io/) Další informace o zápis dotazů.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-149">Use the [Log Analytics query language reference](https://docs.loganalytics.io/) to learn more about writing queries.</span></span>

## <a name="adding-text-and-markdown-sections"></a><span data-ttu-id="e0a5d-150">Přidávání textu a části Markdownu</span><span class="sxs-lookup"><span data-stu-id="e0a5d-150">Adding text and Markdown sections</span></span>

<span data-ttu-id="e0a5d-151">Přidávání záhlaví, vysvětlení a komentáře k sešity pomáhá zapněte sadu tabulky a grafy do komentáře.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-151">Adding headings, explanations, and commentary to your workbooks helps turn a set of tables and charts into a narrative.</span></span> <span data-ttu-id="e0a5d-152">Části textu v podpoře sešity [syntaxe Markdownu](https://daringfireball.net/projects/markdown/) pro formátování textu, jako je záhlaví, tučné, kurzíva a seznamy s odrážkami.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-152">Text sections in workbooks support the [Markdown syntax](https://daringfireball.net/projects/markdown/) for text formatting, like headings, bold, italics, and bulleted lists.</span></span>

<span data-ttu-id="e0a5d-153">Přidat oddíl text k sešitu, použijte **přidat text** tlačítko v dolní části sešitu nebo v dolní části všechny části.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-153">To add a text section to your workbook, use the **Add text** button at the bottom of the workbook, or at the bottom of any section.</span></span>

## <a name="saving-and-sharing-workbooks-with-your-team"></a><span data-ttu-id="e0a5d-154">Ukládání a sdílení sešitů se svým týmem</span><span class="sxs-lookup"><span data-stu-id="e0a5d-154">Saving and sharing workbooks with your team</span></span>

<span data-ttu-id="e0a5d-155">Sešity jsou uloženy v rámci prostředek Application Insights, buď v **Moje sestavy** oddíl, který je soukromý nebo ve **sdílené sestavy** oddíl, který je přístupné všem uživatelům přístup k prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-155">Workbooks are saved within an Application Insights resource, either in the **My Reports** section that's private to you or in the **Shared Reports** section that's accessible to everyone with access to the Application Insights resource.</span></span> <span data-ttu-id="e0a5d-156">Chcete-li zobrazit všechny soubory v prostředku, klikněte na tlačítko **otevřete** tlačítka na panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-156">To view all the workbooks in the resource, click the **Open** button in the action bar.</span></span>

<span data-ttu-id="e0a5d-157">Chcete-li sdílet sešit, který je aktuálně v **Moje sestavy**:</span><span class="sxs-lookup"><span data-stu-id="e0a5d-157">To share a workbook that's currently in **My Reports**:</span></span>

1. <span data-ttu-id="e0a5d-158">Klikněte na tlačítko **otevřete** na panelu akcí</span><span class="sxs-lookup"><span data-stu-id="e0a5d-158">Click **Open** in the action bar</span></span>
2. <span data-ttu-id="e0a5d-159">Klikněte na tlačítko "..." vedle sešit, který chcete sdílet</span><span class="sxs-lookup"><span data-stu-id="e0a5d-159">Click the "..." button beside the workbook you want to share</span></span>
3. <span data-ttu-id="e0a5d-160">Klikněte na tlačítko **přesunout do sdílené sestavy**.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-160">Click **Move to Shared Reports**.</span></span>

<span data-ttu-id="e0a5d-161">Chcete-li sdílet sešit s odkazem nebo e-mailem, klikněte na tlačítko **sdílet** na panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-161">To share a workbook with a link or via email, click **Share** in the action bar.</span></span> <span data-ttu-id="e0a5d-162">Uvědomte si, že příjemci propojení nutné přístup k tomuto prostředku na portálu Azure k zobrazení sešitu.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-162">Keep in mind that recipients of the link need access to this resource in the Azure portal to view the workbook.</span></span> <span data-ttu-id="e0a5d-163">Chcete-li úpravy, příjemci potřebovat aspoň oprávnění přispěvatele pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-163">To make edits, recipients need at least Contributor permissions for the resource.</span></span>

<span data-ttu-id="e0a5d-164">Chcete-li připnout odkaz k sešitu na řídicí panel služby Azure:</span><span class="sxs-lookup"><span data-stu-id="e0a5d-164">To pin a link to a workbook to an Azure Dashboard:</span></span>

1. <span data-ttu-id="e0a5d-165">Klikněte na tlačítko **otevřete** na panelu akcí</span><span class="sxs-lookup"><span data-stu-id="e0a5d-165">Click **Open** in the action bar</span></span>
2. <span data-ttu-id="e0a5d-166">Klikněte na tlačítko "..." vedle sešit, který chcete připnout</span><span class="sxs-lookup"><span data-stu-id="e0a5d-166">Click the "..." button beside the workbook you want to pin</span></span>
3. <span data-ttu-id="e0a5d-167">Klikněte na tlačítko **připnout na řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-167">Click **Pin to dashboard**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0a5d-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0a5d-168">Next steps</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0a5d-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0a5d-169">Next steps</span></span>
- <span data-ttu-id="e0a5d-170">Pokud chcete povolit použití možnosti, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="e0a5d-170">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="e0a5d-171">Pokud jste již odeslat vlastní události nebo zobrazení stránky, prozkoumejte využití nástroje se dozvíte, jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="e0a5d-171">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    - [<span data-ttu-id="e0a5d-172">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="e0a5d-172">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="e0a5d-173">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="e0a5d-173">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="e0a5d-174">Uchování</span><span class="sxs-lookup"><span data-stu-id="e0a5d-174">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="e0a5d-175">Toky uživatele</span><span class="sxs-lookup"><span data-stu-id="e0a5d-175">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="e0a5d-176">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="e0a5d-176">Add user context</span></span>](app-insights-usage-send-user-context.md)
    
