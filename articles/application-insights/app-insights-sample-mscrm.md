---
title: Aplikace Microsoft Dynamics CRM a Azure Application Insights | Microsoft Docs
description: "Získáte telemetrická data z Microsoft Dynamics CRM Online pomocí Application Insights. Postup instalace, získávání dat, vizualizace a export."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: bwren
ms.openlocfilehash: a9593d5f198e05db80451a599428a296ed02e781
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a><span data-ttu-id="30993-104">Návod: Povolení Telemetrie pro aplikaci Microsoft Dynamics CRM Online pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="30993-104">Walkthrough: Enabling Telemetry for Microsoft Dynamics CRM Online using Application Insights</span></span>
<span data-ttu-id="30993-105">Tento článek ukazuje, jak získat telemetrická data z [Microsoft Dynamics CRM Online](https://www.dynamics.com/) pomocí [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="30993-105">This article shows you how to get telemetry data from [Microsoft Dynamics CRM Online](https://www.dynamics.com/) using [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span></span> <span data-ttu-id="30993-106">Budeme zabývat dokončení proces přidávání skript Application Insights do vaší aplikace, zaznamenávání dat a vizualizace dat sady.</span><span class="sxs-lookup"><span data-stu-id="30993-106">We’ll walk through the complete process of adding Application Insights script to your application, capturing data, and data visualization.</span></span>

> [!NOTE]
> <span data-ttu-id="30993-107">[Procházet ukázkové řešení](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="30993-107">[Browse the sample solution](https://dynamicsandappinsights.codeplex.com/).</span></span>
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a><span data-ttu-id="30993-108">Přidejte Application Insights do nové nebo stávající instance CRM Online</span><span class="sxs-lookup"><span data-stu-id="30993-108">Add Application Insights to new or existing CRM Online instance</span></span>
<span data-ttu-id="30993-109">Do monitorování vaší aplikace, přidejte Application Insights SDK do aplikace.</span><span class="sxs-lookup"><span data-stu-id="30993-109">To monitor your application, you add an Application Insights SDK to your application.</span></span> <span data-ttu-id="30993-110">Sada SDK odesílá telemetrii do [portál Application Insights](https://portal.azure.com), kde můžete používat naše efektivní analýzu a diagnostické nástroje, nebo exportovat data do úložiště.</span><span class="sxs-lookup"><span data-stu-id="30993-110">The SDK sends telemetry to the [Application Insights portal](https://portal.azure.com), where you can use our powerful analysis and diagnostic tools, or export the data to storage.</span></span>

### <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="30993-111">Vytvořte prostředek Application Insights v Azure</span><span class="sxs-lookup"><span data-stu-id="30993-111">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="30993-112">Získat [účet ve službě Microsoft Azure](http://azure.com/pricing).</span><span class="sxs-lookup"><span data-stu-id="30993-112">Get [an account in Microsoft Azure](http://azure.com/pricing).</span></span> 
2. <span data-ttu-id="30993-113">Přihlaste se k [portál Azure](https://portal.azure.com) a přidat nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="30993-113">Sign into the [Azure portal](https://portal.azure.com) and add a new Application Insights resource.</span></span> <span data-ttu-id="30993-114">Toto je, kde bude zpracována a zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="30993-114">This is where your data will be processed and displayed.</span></span>
   
    ![Klikněte na tlačítko +, služby pro vývojáře, Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    <span data-ttu-id="30993-116">Vyberte jako typ aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="30993-116">Choose ASP.NET as the application type.</span></span>
3. <span data-ttu-id="30993-117">Otevřete stránku Začínáme a otevřete "monitorování a diagnostikovat na straně klienta".</span><span class="sxs-lookup"><span data-stu-id="30993-117">Open the Getting Started page and open "Monitor and diagnose client side".</span></span>
   
    ![Fragment kódu pro vložení do webové stránky](./media/app-insights-sample-mscrm/03.png)

<span data-ttu-id="30993-119">**Znaková stránka nechat otevřený** zatímco pracujete na další krok v jiném okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="30993-119">**Keep the code page open** while you do the next step in another browser window.</span></span> <span data-ttu-id="30993-120">Kód budete potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="30993-120">You'll need the code soon.</span></span> 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a><span data-ttu-id="30993-121">Vytvořte prostředek JavaScript webové aplikace Microsoft Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="30993-121">Create a JavaScript web resource in Microsoft Dynamics CRM</span></span>
1. <span data-ttu-id="30993-122">Otevřete CRM Online instance a přihlaste se s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="30993-122">Open your CRM Online instance and login with administrator privileges.</span></span>
2. <span data-ttu-id="30993-123">Otevřete Microsoft Dynamics CRM nastavení, vlastní nastavení, přizpůsobení systému</span><span class="sxs-lookup"><span data-stu-id="30993-123">Open Microsoft Dynamics CRM Settings, Customizations, Customize the System</span></span>
   
    ![Nastavení aplikace Microsoft Dynamics CRM](./media/app-insights-sample-mscrm/04.png)
   
    ![Nastavení > Přizpůsobení](./media/app-insights-sample-mscrm/05.png)

    ![Přizpůsobení možnost systému](./media/app-insights-sample-mscrm/06.png)

1. <span data-ttu-id="30993-127">Vytvořte prostředek JavaScript.</span><span class="sxs-lookup"><span data-stu-id="30993-127">Create a JavaScript resource.</span></span>
   
    ![Dialogové okno Nový webový prostředek](./media/app-insights-sample-mscrm/07.png)
   
    <span data-ttu-id="30993-129">Zadejte jeho název, vyberte **skriptu (JScript)** a otevřete textový editor.</span><span class="sxs-lookup"><span data-stu-id="30993-129">Give it a name, select **Script (JScript)** and open the text editor.</span></span>
   
    ![Otevřete textový editor](./media/app-insights-sample-mscrm/08.png)
2. <span data-ttu-id="30993-131">Zkopírujte kód z Application Insights.</span><span class="sxs-lookup"><span data-stu-id="30993-131">Copy the code from Application Insights.</span></span> <span data-ttu-id="30993-132">Při kopírování nezapomeňte ignorovat značek skriptu.</span><span class="sxs-lookup"><span data-stu-id="30993-132">While copying make sure to ignore script tags.</span></span> <span data-ttu-id="30993-133">Naleznete níže – snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="30993-133">Refer below screenshot:</span></span>
   
    ![Nastavte klíč instrumentace](./media/app-insights-sample-mscrm/09.png)
   
    <span data-ttu-id="30993-135">Tento kód obsahuje klíč instrumentace, který identifikuje prostředek vaší aplikace statistiky.</span><span class="sxs-lookup"><span data-stu-id="30993-135">The code includes the instrumentation key that identifies your Application insights resource.</span></span>
3. <span data-ttu-id="30993-136">Uložte a publikovat.</span><span class="sxs-lookup"><span data-stu-id="30993-136">Save and publish.</span></span>
   
    ![Uložte a publikování](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a><span data-ttu-id="30993-138">Nástrojích formulářů</span><span class="sxs-lookup"><span data-stu-id="30993-138">Instrument Forms</span></span>
1. <span data-ttu-id="30993-139">V aplikaci Microsoft CRM Online otevřete formulář účtu</span><span class="sxs-lookup"><span data-stu-id="30993-139">In Microsoft CRM Online, open the Account form</span></span>
   
    ![Účet formuláře](./media/app-insights-sample-mscrm/11.png)
2. <span data-ttu-id="30993-141">Otevřete vlastnosti formuláře</span><span class="sxs-lookup"><span data-stu-id="30993-141">Open the form Properties</span></span>
   
    ![Vlastnosti formuláře](./media/app-insights-sample-mscrm/12.png)
3. <span data-ttu-id="30993-143">Přidání webové prostředky JavaScript, který jste vytvořili</span><span class="sxs-lookup"><span data-stu-id="30993-143">Add the JavaScript web resource that you created</span></span>
   
    ![Nabídka Přidat](./media/app-insights-sample-mscrm/13.png)
   
    ![Přidání webové prostředky](./media/app-insights-sample-mscrm/14.png)
4. <span data-ttu-id="30993-146">Uložte a publikujte vlastní nastavení formuláře.</span><span class="sxs-lookup"><span data-stu-id="30993-146">Save and publish your form customizations.</span></span>

## <a name="metrics-captured"></a><span data-ttu-id="30993-147">Metriky zachycení</span><span class="sxs-lookup"><span data-stu-id="30993-147">Metrics captured</span></span>
<span data-ttu-id="30993-148">Nyní jste nastavili zachycování telemetrie pro daný formulář.</span><span class="sxs-lookup"><span data-stu-id="30993-148">You have now set up telemetry capture for the form.</span></span> <span data-ttu-id="30993-149">Vždy, když se používají data se odešlou do zdroje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="30993-149">Whenever it is used, data will be sent to your Application Insights resource.</span></span>

<span data-ttu-id="30993-150">Tady jsou vzorků dat, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="30993-150">Here are samples of the data that you'll see.</span></span>

#### <a name="application-health"></a><span data-ttu-id="30993-151">Stav aplikace</span><span class="sxs-lookup"><span data-stu-id="30993-151">Application health</span></span>
![Příklad čas načítání stránky](./media/app-insights-sample-mscrm/15.png)

![Příklad stránky zobrazení grafu](./media/app-insights-sample-mscrm/16.png)

<span data-ttu-id="30993-154">Výjimky prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="30993-154">Browser exceptions:</span></span>

![Grafu výjimek prohlížeče](./media/app-insights-sample-mscrm/17.png)

<span data-ttu-id="30993-156">Klikněte na graf zobrazíte další podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="30993-156">Click the chart to get more detail:</span></span>

![Seznamu výjimek](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a><span data-ttu-id="30993-158">Využití</span><span class="sxs-lookup"><span data-stu-id="30993-158">Usage</span></span>
![Uživatelů, relací a zobrazení stránek](./media/app-insights-sample-mscrm/19.png)

![Sesion grafy](./media/app-insights-sample-mscrm/20.png)

![Verze prohlížeče](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a><span data-ttu-id="30993-162">Prohlížeče</span><span class="sxs-lookup"><span data-stu-id="30993-162">Browsers</span></span>
![Rozdělení čas načítání stránky](./media/app-insights-sample-mscrm/22.png)

![Počet relací podle verze prohlížeče](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a><span data-ttu-id="30993-165">Informace o zeměpisné poloze</span><span class="sxs-lookup"><span data-stu-id="30993-165">Geolocation</span></span>
![Počet relací podle země](./media/app-insights-sample-mscrm/24.png)

![Relace a uživatelů podle země](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a><span data-ttu-id="30993-168">Požadavek na uvnitř stránku zobrazení</span><span class="sxs-lookup"><span data-stu-id="30993-168">Inside page view request</span></span>
![Stránka zobrazení souhrnu](./media/app-insights-sample-mscrm/26.png)

![Vyhledávání na stránky zobrazení událostí](./media/app-insights-sample-mscrm/27.png)

![Podobně jako zobrazení stránky](./media/app-insights-sample-mscrm/28.png)

![Zobrazení vlastností stránky](./media/app-insights-sample-mscrm/29.png)

![Stránky na relaci](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a><span data-ttu-id="30993-174">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="30993-174">Sample code</span></span>
<span data-ttu-id="30993-175">[Procházet ukázkový kód](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="30993-175">[Browse the sample code](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="power-bi"></a><span data-ttu-id="30993-176">Power BI</span><span class="sxs-lookup"><span data-stu-id="30993-176">Power BI</span></span>
<span data-ttu-id="30993-177">Můžete provést i hlubší analysis, pokud jste [export dat k Microsoft Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="30993-177">You can do even deeper analysis if you [export the data to Microsoft Power BI](app-insights-export-power-bi.md).</span></span>

## <a name="sample-microsoft-dynamics-crm-solution"></a><span data-ttu-id="30993-178">Ukázkové aplikace Microsoft Dynamics CRM řešení</span><span class="sxs-lookup"><span data-stu-id="30993-178">Sample Microsoft Dynamics CRM Solution</span></span>
<span data-ttu-id="30993-179">[Zde je ukázka řešení implementována v aplikaci Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="30993-179">[Here is the sample solution implemented in Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="learn-more"></a><span data-ttu-id="30993-180">Další informace</span><span class="sxs-lookup"><span data-stu-id="30993-180">Learn more</span></span>
* [<span data-ttu-id="30993-181">Co je Application Insights?</span><span class="sxs-lookup"><span data-stu-id="30993-181">What is Application Insights?</span></span>](app-insights-overview.md)
* [<span data-ttu-id="30993-182">Application Insights pro webové stránky</span><span class="sxs-lookup"><span data-stu-id="30993-182">Application Insights for web pages</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="30993-183">Další ukázky a návody</span><span class="sxs-lookup"><span data-stu-id="30993-183">More samples and walkthroughs</span></span>](app-insights-code-samples.md)
