---
title: aaaExport tooPower BI z Application Insights | Microsoft Docs
description: "Analytické dotazy lze zobrazit v Power BI."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 6668cd7f4e0fbf41695972617f5f8ec207356659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="feed-power-bi-from-application-insights"></a><span data-ttu-id="eefce-103">Kanálu Power BI z Application Insights</span><span class="sxs-lookup"><span data-stu-id="eefce-103">Feed Power BI from Application Insights</span></span>
<span data-ttu-id="eefce-104">[Power BI](http://www.powerbi.com/) je sada nástrojů obchodní analýzy, které vám pomohou analyzovat data a sdílet uplatnitelné.</span><span class="sxs-lookup"><span data-stu-id="eefce-104">[Power BI](http://www.powerbi.com/) is a suite of business analytics tools that help you analyze data and share insights.</span></span> <span data-ttu-id="eefce-105">Bohaté řídicí panely jsou k dispozici na každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="eefce-105">Rich dashboards are available on every device.</span></span> <span data-ttu-id="eefce-106">Můžete kombinovat data z mnoha zdrojů, včetně analytické dotazy z [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eefce-106">You can combine data from many sources, including Analytics queries from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="eefce-107">Export dat tooPower Application Insights BI tři doporučené způsoby.</span><span class="sxs-lookup"><span data-stu-id="eefce-107">There are three recommended methods of exporting Application Insights data tooPower BI.</span></span> <span data-ttu-id="eefce-108">Můžete je používat samostatně nebo společně.</span><span class="sxs-lookup"><span data-stu-id="eefce-108">You can use them separately or together.</span></span>

* <span data-ttu-id="eefce-109">[**Power BI adaptér** ](#power-pi-adapter) -nastavit kompletní řídicí panel telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="eefce-109">[**Power BI adapter**](#power-pi-adapter) - set up a complete dashboard of telemetry from your app.</span></span> <span data-ttu-id="eefce-110">Předdefinovaná Hello sadu grafy, ale můžete přidat vlastní dotazy z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="eefce-110">hello set of charts is predefined, but you can add your own queries from any other sources.</span></span>
* <span data-ttu-id="eefce-111">[**Export analytické dotazy** ](#export-analytics-queries) -zápisu žádný dotaz chcete pomocí Analytics a exportovat je tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="eefce-111">[**Export Analytics queries**](#export-analytics-queries) - write any query you want using Analytics, and export it tooPower BI.</span></span> <span data-ttu-id="eefce-112">Tento dotaz můžete umístit na řídicím panelu společně s dalšími daty.</span><span class="sxs-lookup"><span data-stu-id="eefce-112">You can place this query on a dashboard along with any other data.</span></span>
* <span data-ttu-id="eefce-113">[**Průběžné exportu a Stream Analytics** ](app-insights-export-stream-analytics.md) – to zahrnuje další pracovní tooset nahoru.</span><span class="sxs-lookup"><span data-stu-id="eefce-113">[**Continuous export and Stream Analytics**](app-insights-export-stream-analytics.md) - This involves more work tooset up.</span></span> <span data-ttu-id="eefce-114">Je užitečné, pokud chcete svá data, tookeep dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="eefce-114">It is useful if you want tookeep your data for long periods.</span></span> <span data-ttu-id="eefce-115">V opačném hello jiné metody se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="eefce-115">Otherwise, hello other methods are recommended.</span></span>

## <a name="power-bi-adapter"></a><span data-ttu-id="eefce-116">Adaptér Power BI</span><span class="sxs-lookup"><span data-stu-id="eefce-116">Power BI adapter</span></span>
<span data-ttu-id="eefce-117">Tato metoda vytvoří kompletní řídicí panel telemetrie.</span><span class="sxs-lookup"><span data-stu-id="eefce-117">This method creates a complete dashboard of telemetry for you.</span></span> <span data-ttu-id="eefce-118">Předdefinovaná Hello počáteční datové sady, ale můžete přidat další tooit data.</span><span class="sxs-lookup"><span data-stu-id="eefce-118">hello initial data set is predefined, but you can add more data tooit.</span></span>

### <a name="get-hello-adapter"></a><span data-ttu-id="eefce-119">Získat adaptér hello</span><span class="sxs-lookup"><span data-stu-id="eefce-119">Get hello adapter</span></span>
1. <span data-ttu-id="eefce-120">Přihlaste se příliš[Power BI](https://app.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="eefce-120">Sign in too[Power BI](https://app.powerbi.com/).</span></span>
2. <span data-ttu-id="eefce-121">Otevřete **získat Data**, **služby**, **Application Insights**</span><span class="sxs-lookup"><span data-stu-id="eefce-121">Open **Get Data**, **Services**, **Application Insights**</span></span>
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. <span data-ttu-id="eefce-123">Zadejte podrobnosti o hello prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="eefce-123">Provide hello details of your Application Insights resource.</span></span>
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. <span data-ttu-id="eefce-125">Počkejte, než minutu nebo dvě pro toobe hello data importovat.</span><span class="sxs-lookup"><span data-stu-id="eefce-125">Wait a minute or two for hello data toobe imported.</span></span>
   
    ![Adaptér Power BI](./media/app-insights-export-power-bi/010.png)

<span data-ttu-id="eefce-127">Můžete upravit řídicí panel hello, kombinace hello Application Insights grafy s u jiných zdrojů a se analytické dotazy.</span><span class="sxs-lookup"><span data-stu-id="eefce-127">You can edit hello dashboard, combining hello Application Insights charts with those of other sources, and with Analytics queries.</span></span> <span data-ttu-id="eefce-128">Je Galerie vizualizace můžete získat více grafů, kde každý graf obsahuje parametry, které můžete zadat.</span><span class="sxs-lookup"><span data-stu-id="eefce-128">There's a visualization gallery where you can get more charts, and each chart has a parameters you can set.</span></span>

<span data-ttu-id="eefce-129">Po importu počáteční hello, hello řídicí panel a sestavy hello pokračovat tooupdate denně.</span><span class="sxs-lookup"><span data-stu-id="eefce-129">After hello initial import, hello dashboard and hello reports continue tooupdate daily.</span></span> <span data-ttu-id="eefce-130">Můžete ovládat hello plán aktualizace pro datovou sadu hello.</span><span class="sxs-lookup"><span data-stu-id="eefce-130">You can control hello refresh schedule on hello dataset.</span></span>

## <a name="export-analytics-queries"></a><span data-ttu-id="eefce-131">Export analytické dotazy</span><span class="sxs-lookup"><span data-stu-id="eefce-131">Export Analytics queries</span></span>
<span data-ttu-id="eefce-132">Tato trasa vám umožní toowrite žádné Analytics dotazu vám líbilo a poté exportujte tento řídicí panel Power BI tooa.</span><span class="sxs-lookup"><span data-stu-id="eefce-132">This route allows you toowrite any Analytics query you like, and then export that tooa Power BI dashboard.</span></span> <span data-ttu-id="eefce-133">(Můžete přidat řídicí panel toohello vytvořený adaptér hello.)</span><span class="sxs-lookup"><span data-stu-id="eefce-133">(You can add toohello dashboard created by hello adapter.)</span></span>

### <a name="one-time-install-power-bi-desktop"></a><span data-ttu-id="eefce-134">Jednou: Nainstalujte Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="eefce-134">One time: install Power BI Desktop</span></span>
<span data-ttu-id="eefce-135">tooimport Application Insights dotaz, použít hello plochy verze Power BI.</span><span class="sxs-lookup"><span data-stu-id="eefce-135">tooimport your Application Insights query, you use hello desktop version of Power BI.</span></span> <span data-ttu-id="eefce-136">Ale pak ho můžete publikovat toohello web nebo tooyour prostoru cloudu Power BI.</span><span class="sxs-lookup"><span data-stu-id="eefce-136">But then you can publish it toohello web or tooyour Power BI cloud workspace.</span></span> 

<span data-ttu-id="eefce-137">Nainstalujte [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span><span class="sxs-lookup"><span data-stu-id="eefce-137">Install [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span></span>

### <a name="export-an-analytics-query"></a><span data-ttu-id="eefce-138">Export dotaz Analytics</span><span class="sxs-lookup"><span data-stu-id="eefce-138">Export an Analytics query</span></span>
1. <span data-ttu-id="eefce-139">[Otevřete analýzy a napsat dotaz](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="eefce-139">[Open Analytics and write your query](app-insights-analytics-tour.md).</span></span>
2. <span data-ttu-id="eefce-140">Testování a dotaz hello Upřesnit, až budete spokojeni s výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="eefce-140">Test and refine hello query until you're happy with hello results.</span></span>

   <span data-ttu-id="eefce-141">**Ujistěte se, že hello dotazů běží správně v Analytics předtím, než ho exportovat.**</span><span class="sxs-lookup"><span data-stu-id="eefce-141">**Make sure that hello query runs correctly in Analytics before you export it.**</span></span>
3. <span data-ttu-id="eefce-142">Na hello **exportovat** nabídce zvolte **Power BI (M)**.</span><span class="sxs-lookup"><span data-stu-id="eefce-142">On hello **Export** menu, choose **Power BI (M)**.</span></span> <span data-ttu-id="eefce-143">Uložte soubor textu hello.</span><span class="sxs-lookup"><span data-stu-id="eefce-143">Save hello text file.</span></span>
   
    ![Export dotazu Power BI](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. <span data-ttu-id="eefce-145">V Power BI Desktop vyberte **načíst Data, prázdné dotazu** a pak v hello dotazu editor, v části **zobrazení** vyberte **Advanced Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="eefce-145">In Power BI Desktop select **Get Data, Blank Query** and then in hello query editor, under **View** select **Advanced Query Editor**.</span></span>

    <span data-ttu-id="eefce-146">Skript jazyka M hello exportovat vložení do hello Advanced editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="eefce-146">Paste hello exported M Language script into hello Advanced Query Editor.</span></span>

    ![Rozšířený dotaz editoru](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. <span data-ttu-id="eefce-148">Můžete mít tooprovide pověření tooallow Power BI tooaccess Azure.</span><span class="sxs-lookup"><span data-stu-id="eefce-148">You might have tooprovide credentials tooallow Power BI tooaccess Azure.</span></span> <span data-ttu-id="eefce-149">Použijte toosign 'účet organizace, se pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="eefce-149">Use 'organizational account' toosign in with your Microsoft account.</span></span>
   
    ![Zadejte přihlašovací údaje Azure tooenable Power BI toorun dotazu Application Insights](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    <span data-ttu-id="eefce-151">(Pokud budete potřebovat přihlašovací údaje tooverify hello, použijte příkaz nabídky hello nastavení zdroje dat v hello editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="eefce-151">(If you need tooverify hello credentials, use hello Data Source Settings menu command in hello Query Editor.</span></span> <span data-ttu-id="eefce-152">Opatrní toospecify hello pověření, které používáte pro Azure, což může být liší od přihlašovacích údajů pro Power BI.)</span><span class="sxs-lookup"><span data-stu-id="eefce-152">Take care toospecify hello credentials you use for Azure, which might be different from your credentials for Power BI.)</span></span>
2. <span data-ttu-id="eefce-153">Zvolte vizualizaci dotazu a vyberte hello pole osy x, y a segmentace dimenze.</span><span class="sxs-lookup"><span data-stu-id="eefce-153">Choose a visualization for your query and select hello fields for x-axis, y-axis, and segmenting dimension.</span></span>
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. <span data-ttu-id="eefce-155">Publikujte pracovní prostor sestavy tooyour Power BI cloudu.</span><span class="sxs-lookup"><span data-stu-id="eefce-155">Publish your report tooyour Power BI cloud workspace.</span></span> <span data-ttu-id="eefce-156">Odtud můžete synchronizované verze vložit do jiných webových stránek.</span><span class="sxs-lookup"><span data-stu-id="eefce-156">From there, you can embed a synchronized version into other web pages.</span></span>
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/publish-power-bi.png)
4. <span data-ttu-id="eefce-158">Ručně aktualizujte hello sestavy v intervalech, nebo nastavení plánované aktualizace na stránce Možnosti hello.</span><span class="sxs-lookup"><span data-stu-id="eefce-158">Refresh hello report manually at intervals, or set up a scheduled refresh on hello options page.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="eefce-159">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="eefce-159">Troubleshooting</span></span>

### <a name="401-or-403-unauthorized"></a><span data-ttu-id="eefce-160">401 nebo 403 Neautorizováno</span><span class="sxs-lookup"><span data-stu-id="eefce-160">401 or 403 Unauthorized</span></span> 
<span data-ttu-id="eefce-161">To může dojít, pokud zatím není aktualizovaná refesh tokenu.</span><span class="sxs-lookup"><span data-stu-id="eefce-161">This can happen if your refesh token has not been updated.</span></span> <span data-ttu-id="eefce-162">Opakujte tyto kroky tooensure máte pořád přístup.</span><span class="sxs-lookup"><span data-stu-id="eefce-162">Try these steps tooensure you still have access.</span></span> <span data-ttu-id="eefce-163">Pokud máte přístup a přihlašovací údaje hello refershing nefunguje, otevřete prosím lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="eefce-163">If you do have access and refershing hello credentials does not work, please open a support ticket.</span></span>

1. <span data-ttu-id="eefce-164">Přihlaste se k portálu Azure hello a ujistěte se, že dostanete hello prostředků</span><span class="sxs-lookup"><span data-stu-id="eefce-164">Log into hello Azure Portal and make sure you can access hello resource</span></span>
2. <span data-ttu-id="eefce-165">Zkuste toorefresh hello přihlašovací údaje pro hello řídicí panel</span><span class="sxs-lookup"><span data-stu-id="eefce-165">Try toorefresh hello credentials for hello Dashboard</span></span>

### <a name="502-bad-gateway"></a><span data-ttu-id="eefce-166">502 Chybná brána</span><span class="sxs-lookup"><span data-stu-id="eefce-166">502 Bad Gateway</span></span>
<span data-ttu-id="eefce-167">To je obvykle způsobeno dotazu analýzy, který vrátí příliš mnoho dat.</span><span class="sxs-lookup"><span data-stu-id="eefce-167">This is usually caused by an Analytics query that returns too much data.</span></span> <span data-ttu-id="eefce-168">Měli zkuste použít menší časový rozsah, nebo pomocí hello [před](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) nebo [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) funguje pouze [projektu](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello polí je nutné.</span><span class="sxs-lookup"><span data-stu-id="eefce-168">You should try using a smaller time range or by using hello [ago](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) or [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) functions only [project](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello fields you need.</span></span>

<span data-ttu-id="eefce-169">Pokud snižuje datovou sadu hello pocházejících z dotazu hello Analytics není splňují vaše požadavky měli byste zvážit použití hello [rozhraní API](https://dev.applicationinsights.io/documentation/overview) toopull větší datové sady.</span><span class="sxs-lookup"><span data-stu-id="eefce-169">If reducing hello dataset coming from hello Analytics query doesn't meet your requirements you should consider using hello [API](https://dev.applicationinsights.io/documentation/overview) toopull a larger dataset.</span></span> <span data-ttu-id="eefce-170">Tady jsou pokyny, jak exportovat tooconvert hello M-Query toouse hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="eefce-170">Here are instructions on how tooconvert hello M-Query export toouse hello API.</span></span>

1. <span data-ttu-id="eefce-171">Vytvoření [klíč rozhraní API](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span><span class="sxs-lookup"><span data-stu-id="eefce-171">Create an [API Key](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span></span>
2. <span data-ttu-id="eefce-172">Aktualizace hello Power BI M skript, který jste exportovali z Analytics nahrazením hello ARM adresu URL s rozhraním API AI (viz následující příklad)</span><span class="sxs-lookup"><span data-stu-id="eefce-172">Update hello Power BI M script that you exported from Analytics by replacing hello ARM URL with AI API (see example below)</span></span>
   * <span data-ttu-id="eefce-173">Nahraďte **https://management.azure.com/subscriptions/...**</span><span class="sxs-lookup"><span data-stu-id="eefce-173">Replace **https://management.azure.com/subscriptions/...**</span></span>
   * <span data-ttu-id="eefce-174">s **https://api.applicationinsights.io/beta/apps/...**</span><span class="sxs-lookup"><span data-stu-id="eefce-174">with, **https://api.applicationinsights.io/beta/apps/...**</span></span>
3. <span data-ttu-id="eefce-175">Nakonec aktualizujte toobasic přihlašovacích údajů a používání vašeho klíče rozhraní API</span><span class="sxs-lookup"><span data-stu-id="eefce-175">Finally, update credentials toobasic, and use your API Key</span></span>
  

<span data-ttu-id="eefce-176">**Existující skriptu**</span><span class="sxs-lookup"><span data-stu-id="eefce-176">**Existing Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
<span data-ttu-id="eefce-177">**Aktualizované skriptu**</span><span class="sxs-lookup"><span data-stu-id="eefce-177">**Updated Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a><span data-ttu-id="eefce-178">O vzorkování</span><span class="sxs-lookup"><span data-stu-id="eefce-178">About sampling</span></span>
<span data-ttu-id="eefce-179">Pokud vaše aplikace odešle velké množství dat, může funkce adaptivního vzorkování hello pracovat a odesílat pouze procento vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="eefce-179">If your application sends a lot of data, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> <span data-ttu-id="eefce-180">Dobrý den, je-li hodnotu true, pokud jste ručně nastavili vzorkování v hello SDK nebo na přijímání stejné.</span><span class="sxs-lookup"><span data-stu-id="eefce-180">hello same is true if you have manually set sampling either in hello SDK or on ingestion.</span></span> [<span data-ttu-id="eefce-181">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="eefce-181">Learn more about sampling.</span></span>](app-insights-sampling.md)


## <a name="next-steps"></a><span data-ttu-id="eefce-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eefce-182">Next steps</span></span>
* [<span data-ttu-id="eefce-183">Power BI - Další informace</span><span class="sxs-lookup"><span data-stu-id="eefce-183">Power BI - Learn</span></span>](http://www.powerbi.com/learning/)
* [<span data-ttu-id="eefce-184">Analýza kurzu</span><span class="sxs-lookup"><span data-stu-id="eefce-184">Analytics tutorial</span></span>](app-insights-analytics-tour.md)

