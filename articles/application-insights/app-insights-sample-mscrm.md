---
title: aaaMicrosoft Dynamics CRM, Azure Application Insights | Microsoft Docs
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
ms.openlocfilehash: a39398060d6553fb18a26c101f085b7d87443636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a><span data-ttu-id="55f48-104">Návod: Povolení Telemetrie pro aplikaci Microsoft Dynamics CRM Online pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="55f48-104">Walkthrough: Enabling Telemetry for Microsoft Dynamics CRM Online using Application Insights</span></span>
<span data-ttu-id="55f48-105">Tento článek ukazuje, jak tooget telemetrická data z [Microsoft Dynamics CRM Online](https://www.dynamics.com/) pomocí [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="55f48-105">This article shows you how tooget telemetry data from [Microsoft Dynamics CRM Online](https://www.dynamics.com/) using [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span></span> <span data-ttu-id="55f48-106">Projdeme hello dokončení procesu přidání Application Insights skriptu tooyour aplikace, zaznamenávání dat a vizualizace dat sady.</span><span class="sxs-lookup"><span data-stu-id="55f48-106">We’ll walk through hello complete process of adding Application Insights script tooyour application, capturing data, and data visualization.</span></span>

> [!NOTE]
> <span data-ttu-id="55f48-107">[Procházet hello ukázkové řešení](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="55f48-107">[Browse hello sample solution](https://dynamicsandappinsights.codeplex.com/).</span></span>
> 
> 

## <a name="add-application-insights-toonew-or-existing-crm-online-instance"></a><span data-ttu-id="55f48-108">Přidejte Application Insights toonew nebo existující instanci CRM Online</span><span class="sxs-lookup"><span data-stu-id="55f48-108">Add Application Insights toonew or existing CRM Online instance</span></span>
<span data-ttu-id="55f48-109">toomonitor vaší aplikace, přidání aplikace tooyour Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="55f48-109">toomonitor your application, you add an Application Insights SDK tooyour application.</span></span> <span data-ttu-id="55f48-110">Hello SDK odesílá telemetrii toohello [portál Application Insights](https://portal.azure.com), kde můžete používat naše efektivní analýzu a diagnostické nástroje, nebo exportovat hello data toostorage.</span><span class="sxs-lookup"><span data-stu-id="55f48-110">hello SDK sends telemetry toohello [Application Insights portal](https://portal.azure.com), where you can use our powerful analysis and diagnostic tools, or export hello data toostorage.</span></span>

### <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="55f48-111">Vytvořte prostředek Application Insights v Azure</span><span class="sxs-lookup"><span data-stu-id="55f48-111">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="55f48-112">Získat [účet ve službě Microsoft Azure](http://azure.com/pricing).</span><span class="sxs-lookup"><span data-stu-id="55f48-112">Get [an account in Microsoft Azure](http://azure.com/pricing).</span></span> 
2. <span data-ttu-id="55f48-113">Přihlaste se k hello [portál Azure](https://portal.azure.com) a přidat nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="55f48-113">Sign into hello [Azure portal](https://portal.azure.com) and add a new Application Insights resource.</span></span> <span data-ttu-id="55f48-114">Toto je, kde bude zpracována a zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="55f48-114">This is where your data will be processed and displayed.</span></span>
   
    ![Klikněte na tlačítko +, služby pro vývojáře, Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    <span data-ttu-id="55f48-116">Vyberte jako typ aplikace hello ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="55f48-116">Choose ASP.NET as hello application type.</span></span>
3. <span data-ttu-id="55f48-117">Otevřete stránku Začínáme hello a otevřete "monitorování a diagnostikovat na straně klienta".</span><span class="sxs-lookup"><span data-stu-id="55f48-117">Open hello Getting Started page and open "Monitor and diagnose client side".</span></span>
   
    ![Fragment kódu pro vložení do webové stránky](./media/app-insights-sample-mscrm/03.png)

<span data-ttu-id="55f48-119">**Nechat otevřený hello znaková stránka** při hello další krok v jiném okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="55f48-119">**Keep hello code page open** while you do hello next step in another browser window.</span></span> <span data-ttu-id="55f48-120">Budete potřebovat kód hello brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="55f48-120">You'll need hello code soon.</span></span> 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a><span data-ttu-id="55f48-121">Vytvořte prostředek JavaScript webové aplikace Microsoft Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="55f48-121">Create a JavaScript web resource in Microsoft Dynamics CRM</span></span>
1. <span data-ttu-id="55f48-122">Otevřete CRM Online instance a přihlaste se s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="55f48-122">Open your CRM Online instance and login with administrator privileges.</span></span>
2. <span data-ttu-id="55f48-123">Otevřete Microsoft Dynamics CRM nastavení, přizpůsobení, přizpůsobit hello systému</span><span class="sxs-lookup"><span data-stu-id="55f48-123">Open Microsoft Dynamics CRM Settings, Customizations, Customize hello System</span></span>
   
    ![Nastavení aplikace Microsoft Dynamics CRM](./media/app-insights-sample-mscrm/04.png)
   
    ![Nastavení > Přizpůsobení](./media/app-insights-sample-mscrm/05.png)

    ![Přizpůsobení možnost hello systému](./media/app-insights-sample-mscrm/06.png)

1. <span data-ttu-id="55f48-127">Vytvořte prostředek JavaScript.</span><span class="sxs-lookup"><span data-stu-id="55f48-127">Create a JavaScript resource.</span></span>
   
    ![Dialogové okno Nový webový prostředek](./media/app-insights-sample-mscrm/07.png)
   
    <span data-ttu-id="55f48-129">Zadejte jeho název, vyberte **skriptu (JScript)** a textovém editoru otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="55f48-129">Give it a name, select **Script (JScript)** and open hello text editor.</span></span>
   
    ![Otevřete hello textového editoru](./media/app-insights-sample-mscrm/08.png)
2. <span data-ttu-id="55f48-131">Zkopírujte kód hello z Application Insights.</span><span class="sxs-lookup"><span data-stu-id="55f48-131">Copy hello code from Application Insights.</span></span> <span data-ttu-id="55f48-132">Při kopírování Ujistěte se, že tooignore značek skriptu.</span><span class="sxs-lookup"><span data-stu-id="55f48-132">While copying make sure tooignore script tags.</span></span> <span data-ttu-id="55f48-133">Naleznete níže – snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="55f48-133">Refer below screenshot:</span></span>
   
    ![Nastavte klíč instrumentace](./media/app-insights-sample-mscrm/09.png)
   
    <span data-ttu-id="55f48-135">Hello kód obsahuje klíč instrumentace hello, který identifikuje prostředek vaší aplikace statistiky.</span><span class="sxs-lookup"><span data-stu-id="55f48-135">hello code includes hello instrumentation key that identifies your Application insights resource.</span></span>
3. <span data-ttu-id="55f48-136">Uložte a publikovat.</span><span class="sxs-lookup"><span data-stu-id="55f48-136">Save and publish.</span></span>
   
    ![Uložte a publikování](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a><span data-ttu-id="55f48-138">Nástrojích formulářů</span><span class="sxs-lookup"><span data-stu-id="55f48-138">Instrument Forms</span></span>
1. <span data-ttu-id="55f48-139">V aplikaci Microsoft CRM Online otevřete formulář účet hello</span><span class="sxs-lookup"><span data-stu-id="55f48-139">In Microsoft CRM Online, open hello Account form</span></span>
   
    ![Účet formuláře](./media/app-insights-sample-mscrm/11.png)
2. <span data-ttu-id="55f48-141">Otevřít formulář hello vlastnosti</span><span class="sxs-lookup"><span data-stu-id="55f48-141">Open hello form Properties</span></span>
   
    ![Vlastnosti formuláře](./media/app-insights-sample-mscrm/12.png)
3. <span data-ttu-id="55f48-143">Přidat hello JavaScript webové prostředky, který jste vytvořili</span><span class="sxs-lookup"><span data-stu-id="55f48-143">Add hello JavaScript web resource that you created</span></span>
   
    ![Nabídka Přidat](./media/app-insights-sample-mscrm/13.png)
   
    ![Přidat prostředek webové hello](./media/app-insights-sample-mscrm/14.png)
4. <span data-ttu-id="55f48-146">Uložte a publikujte vlastní nastavení formuláře.</span><span class="sxs-lookup"><span data-stu-id="55f48-146">Save and publish your form customizations.</span></span>

## <a name="metrics-captured"></a><span data-ttu-id="55f48-147">Metriky zachycení</span><span class="sxs-lookup"><span data-stu-id="55f48-147">Metrics captured</span></span>
<span data-ttu-id="55f48-148">Nyní jste nastavili zachycování telemetrie pro formulář hello.</span><span class="sxs-lookup"><span data-stu-id="55f48-148">You have now set up telemetry capture for hello form.</span></span> <span data-ttu-id="55f48-149">Vždy, když se používají data odešlou tooyour prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="55f48-149">Whenever it is used, data will be sent tooyour Application Insights resource.</span></span>

<span data-ttu-id="55f48-150">Tady jsou ukázky hello data, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="55f48-150">Here are samples of hello data that you'll see.</span></span>

#### <a name="application-health"></a><span data-ttu-id="55f48-151">Stav aplikace</span><span class="sxs-lookup"><span data-stu-id="55f48-151">Application health</span></span>
![Příklad čas načítání stránky](./media/app-insights-sample-mscrm/15.png)

![Příklad stránky zobrazení grafu](./media/app-insights-sample-mscrm/16.png)

<span data-ttu-id="55f48-154">Výjimky prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="55f48-154">Browser exceptions:</span></span>

![Grafu výjimek prohlížeče](./media/app-insights-sample-mscrm/17.png)

<span data-ttu-id="55f48-156">Klikněte na graf tooget hello podrobněji:</span><span class="sxs-lookup"><span data-stu-id="55f48-156">Click hello chart tooget more detail:</span></span>

![Seznamu výjimek](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a><span data-ttu-id="55f48-158">Využití</span><span class="sxs-lookup"><span data-stu-id="55f48-158">Usage</span></span>
![Uživatelů, relací a zobrazení stránek](./media/app-insights-sample-mscrm/19.png)

![Sesion grafy](./media/app-insights-sample-mscrm/20.png)

![Verze prohlížeče](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a><span data-ttu-id="55f48-162">Prohlížeče</span><span class="sxs-lookup"><span data-stu-id="55f48-162">Browsers</span></span>
![Rozdělení čas načítání stránky](./media/app-insights-sample-mscrm/22.png)

![Počet relací podle verze prohlížeče](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a><span data-ttu-id="55f48-165">Informace o zeměpisné poloze</span><span class="sxs-lookup"><span data-stu-id="55f48-165">Geolocation</span></span>
![Počet relací podle země](./media/app-insights-sample-mscrm/24.png)

![Relace a uživatelů podle země](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a><span data-ttu-id="55f48-168">Požadavek na uvnitř stránku zobrazení</span><span class="sxs-lookup"><span data-stu-id="55f48-168">Inside page view request</span></span>
![Stránka zobrazení souhrnu](./media/app-insights-sample-mscrm/26.png)

![Vyhledávání na stránky zobrazení událostí](./media/app-insights-sample-mscrm/27.png)

![Podobně jako zobrazení stránky](./media/app-insights-sample-mscrm/28.png)

![Zobrazení vlastností stránky](./media/app-insights-sample-mscrm/29.png)

![Stránky na relaci](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a><span data-ttu-id="55f48-174">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="55f48-174">Sample code</span></span>
<span data-ttu-id="55f48-175">[Procházet hello ukázkový kód](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="55f48-175">[Browse hello sample code](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="power-bi"></a><span data-ttu-id="55f48-176">Power BI</span><span class="sxs-lookup"><span data-stu-id="55f48-176">Power BI</span></span>
<span data-ttu-id="55f48-177">Můžete provést i hlubší analysis, pokud jste [exportovat hello data tooMicrosoft Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="55f48-177">You can do even deeper analysis if you [export hello data tooMicrosoft Power BI](app-insights-export-power-bi.md).</span></span>

## <a name="sample-microsoft-dynamics-crm-solution"></a><span data-ttu-id="55f48-178">Ukázkové aplikace Microsoft Dynamics CRM řešení</span><span class="sxs-lookup"><span data-stu-id="55f48-178">Sample Microsoft Dynamics CRM Solution</span></span>
<span data-ttu-id="55f48-179">[Tady je implementována v aplikaci Microsoft Dynamics CRM hello ukázkové řešení](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="55f48-179">[Here is hello sample solution implemented in Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="learn-more"></a><span data-ttu-id="55f48-180">Další informace</span><span class="sxs-lookup"><span data-stu-id="55f48-180">Learn more</span></span>
* [<span data-ttu-id="55f48-181">Co je Application Insights?</span><span class="sxs-lookup"><span data-stu-id="55f48-181">What is Application Insights?</span></span>](app-insights-overview.md)
* [<span data-ttu-id="55f48-182">Application Insights pro webové stránky</span><span class="sxs-lookup"><span data-stu-id="55f48-182">Application Insights for web pages</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="55f48-183">Další ukázky a návody</span><span class="sxs-lookup"><span data-stu-id="55f48-183">More samples and walkthroughs</span></span>](app-insights-code-samples.md)
