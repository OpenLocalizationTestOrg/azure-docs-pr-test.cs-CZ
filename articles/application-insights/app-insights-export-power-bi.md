---
title: "Export do služby Power BI ze služby Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 350a65b1c6432baf258e014c9e63133d2b29e34f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="feed-power-bi-from-application-insights"></a><span data-ttu-id="1c9d4-103">Kanálu Power BI z Application Insights</span><span class="sxs-lookup"><span data-stu-id="1c9d4-103">Feed Power BI from Application Insights</span></span>
<span data-ttu-id="1c9d4-104">[Power BI](http://www.powerbi.com/) je sada nástrojů obchodní analýzy, které vám pomohou analyzovat data a sdílet uplatnitelné.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-104">[Power BI](http://www.powerbi.com/) is a suite of business analytics tools that help you analyze data and share insights.</span></span> <span data-ttu-id="1c9d4-105">Bohaté řídicí panely jsou k dispozici na každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-105">Rich dashboards are available on every device.</span></span> <span data-ttu-id="1c9d4-106">Můžete kombinovat data z mnoha zdrojů, včetně analytické dotazy z [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1c9d4-106">You can combine data from many sources, including Analytics queries from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="1c9d4-107">Existují tři doporučené metody export dat Application Insights do Power BI.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-107">There are three recommended methods of exporting Application Insights data to Power BI.</span></span> <span data-ttu-id="1c9d4-108">Můžete je používat samostatně nebo společně.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-108">You can use them separately or together.</span></span>

* <span data-ttu-id="1c9d4-109">[**Power BI adaptér** ](#power-pi-adapter) -nastavit kompletní řídicí panel telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-109">[**Power BI adapter**](#power-pi-adapter) - set up a complete dashboard of telemetry from your app.</span></span> <span data-ttu-id="1c9d4-110">Předdefinovaná sadu grafy, ale můžete přidat vlastní dotazy z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-110">The set of charts is predefined, but you can add your own queries from any other sources.</span></span>
* <span data-ttu-id="1c9d4-111">[**Export analytické dotazy** ](#export-analytics-queries) -zápisu žádný dotaz chcete pomocí Analytics a exportovat je do Power BI.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-111">[**Export Analytics queries**](#export-analytics-queries) - write any query you want using Analytics, and export it to Power BI.</span></span> <span data-ttu-id="1c9d4-112">Tento dotaz můžete umístit na řídicím panelu společně s dalšími daty.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-112">You can place this query on a dashboard along with any other data.</span></span>
* <span data-ttu-id="1c9d4-113">[**Průběžné exportu a Stream Analytics** ](app-insights-export-stream-analytics.md) – to zahrnuje další práci nastavit.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-113">[**Continuous export and Stream Analytics**](app-insights-export-stream-analytics.md) - This involves more work to set up.</span></span> <span data-ttu-id="1c9d4-114">Je užitečné, pokud chcete zachovat data dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-114">It is useful if you want to keep your data for long periods.</span></span> <span data-ttu-id="1c9d4-115">Doporučuje se, jinak hodnota jiné metody.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-115">Otherwise, the other methods are recommended.</span></span>

## <a name="power-bi-adapter"></a><span data-ttu-id="1c9d4-116">Adaptér Power BI</span><span class="sxs-lookup"><span data-stu-id="1c9d4-116">Power BI adapter</span></span>
<span data-ttu-id="1c9d4-117">Tato metoda vytvoří kompletní řídicí panel telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-117">This method creates a complete dashboard of telemetry for you.</span></span> <span data-ttu-id="1c9d4-118">Předdefinovaná počáteční datovou sadu, ale do něj může přidat další data.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-118">The initial data set is predefined, but you can add more data to it.</span></span>

### <a name="get-the-adapter"></a><span data-ttu-id="1c9d4-119">Získat adaptéru</span><span class="sxs-lookup"><span data-stu-id="1c9d4-119">Get the adapter</span></span>
1. <span data-ttu-id="1c9d4-120">Přihlaste se k [Power BI](https://app.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="1c9d4-120">Sign in to [Power BI](https://app.powerbi.com/).</span></span>
2. <span data-ttu-id="1c9d4-121">Otevřete **získat Data**, **služby**, **Application Insights**</span><span class="sxs-lookup"><span data-stu-id="1c9d4-121">Open **Get Data**, **Services**, **Application Insights**</span></span>
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. <span data-ttu-id="1c9d4-123">Zadejte podrobnosti o prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-123">Provide the details of your Application Insights resource.</span></span>
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. <span data-ttu-id="1c9d4-125">Počkejte, než minutu nebo dvě pro data, která bude importována.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-125">Wait a minute or two for the data to be imported.</span></span>
   
    ![Adaptér Power BI](./media/app-insights-export-power-bi/010.png)

<span data-ttu-id="1c9d4-127">Můžete upravit řídicí panel, kombinace Application Insights grafy s u jiných zdrojů a se analytické dotazy.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-127">You can edit the dashboard, combining the Application Insights charts with those of other sources, and with Analytics queries.</span></span> <span data-ttu-id="1c9d4-128">Je Galerie vizualizace můžete získat více grafů, kde každý graf obsahuje parametry, které můžete zadat.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-128">There's a visualization gallery where you can get more charts, and each chart has a parameters you can set.</span></span>

<span data-ttu-id="1c9d4-129">Po počáteční importu nadále denně aktualizovat řídicí panel a sestavy.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-129">After the initial import, the dashboard and the reports continue to update daily.</span></span> <span data-ttu-id="1c9d4-130">Můžete řídit plán aktualizace na datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-130">You can control the refresh schedule on the dataset.</span></span>

## <a name="export-analytics-queries"></a><span data-ttu-id="1c9d4-131">Export analytické dotazy</span><span class="sxs-lookup"><span data-stu-id="1c9d4-131">Export Analytics queries</span></span>
<span data-ttu-id="1c9d4-132">Tato trasa umožňuje zapsat všechny Analytics dotaz, který chcete a export, na řídicí panel Power BI.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-132">This route allows you to write any Analytics query you like, and then export that to a Power BI dashboard.</span></span> <span data-ttu-id="1c9d4-133">(Můžete přidat na řídicí panel vytvořený adaptér.)</span><span class="sxs-lookup"><span data-stu-id="1c9d4-133">(You can add to the dashboard created by the adapter.)</span></span>

### <a name="one-time-install-power-bi-desktop"></a><span data-ttu-id="1c9d4-134">Jednou: Nainstalujte Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="1c9d4-134">One time: install Power BI Desktop</span></span>
<span data-ttu-id="1c9d4-135">Chcete-li importovat dotaz Application Insights, použijete verze aplikace Power BI.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-135">To import your Application Insights query, you use the desktop version of Power BI.</span></span> <span data-ttu-id="1c9d4-136">Ale pak je můžete publikovat na webu nebo do pracovního prostoru cloudu Power BI.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-136">But then you can publish it to the web or to your Power BI cloud workspace.</span></span> 

<span data-ttu-id="1c9d4-137">Nainstalujte [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span><span class="sxs-lookup"><span data-stu-id="1c9d4-137">Install [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span></span>

### <a name="export-an-analytics-query"></a><span data-ttu-id="1c9d4-138">Export dotaz Analytics</span><span class="sxs-lookup"><span data-stu-id="1c9d4-138">Export an Analytics query</span></span>
1. <span data-ttu-id="1c9d4-139">[Otevřete analýzy a napsat dotaz](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="1c9d4-139">[Open Analytics and write your query](app-insights-analytics-tour.md).</span></span>
2. <span data-ttu-id="1c9d4-140">Testování a upřesnění dotazu, až budete s výsledky spokojeni.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-140">Test and refine the query until you're happy with the results.</span></span>

   <span data-ttu-id="1c9d4-141">**Ujistěte se, že dotaz běží správně v Analytics předtím, než ho exportovat.**</span><span class="sxs-lookup"><span data-stu-id="1c9d4-141">**Make sure that the query runs correctly in Analytics before you export it.**</span></span>
3. <span data-ttu-id="1c9d4-142">Na **exportovat** nabídce zvolte **Power BI (M)**.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-142">On the **Export** menu, choose **Power BI (M)**.</span></span> <span data-ttu-id="1c9d4-143">Uložte tento textový soubor.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-143">Save the text file.</span></span>
   
    ![Export dotazu Power BI](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. <span data-ttu-id="1c9d4-145">V Power BI Desktop vyberte **načíst Data, prázdné dotazu** a poté v editoru dotazů v rámci **zobrazení** vyberte **Advanced Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-145">In Power BI Desktop select **Get Data, Blank Query** and then in the query editor, under **View** select **Advanced Query Editor**.</span></span>

    <span data-ttu-id="1c9d4-146">Vložte do editoru dotazů Advanced exportovaný skript jazyka M.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-146">Paste the exported M Language script into the Advanced Query Editor.</span></span>

    ![Rozšířený dotaz editoru](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. <span data-ttu-id="1c9d4-148">Možná budete muset zadat přihlašovací údaje, aby se mohl Power BI přístup k Azure.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-148">You might have to provide credentials to allow Power BI to access Azure.</span></span> <span data-ttu-id="1c9d4-149">Použít 'účet organizace, a přihlaste se pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-149">Use 'organizational account' to sign in with your Microsoft account.</span></span>
   
    ![Zadejte přihlašovací údaje Azure umožňující Power BI ke spuštění dotazu Application Insights](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    <span data-ttu-id="1c9d4-151">(Pokud potřebujete k ověření přihlašovacích údajů, použijte příkaz nabídky nastavení zdroje dat v editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-151">(If you need to verify the credentials, use the Data Source Settings menu command in the Query Editor.</span></span> <span data-ttu-id="1c9d4-152">Vezměte v potaz a zadejte přihlašovací údaje, které používáte pro Azure, což může být liší od přihlašovacích údajů pro Power BI.)</span><span class="sxs-lookup"><span data-stu-id="1c9d4-152">Take care to specify the credentials you use for Azure, which might be different from your credentials for Power BI.)</span></span>
2. <span data-ttu-id="1c9d4-153">Zvolte vizualizaci dotazu a vyberte pole pro osy x, y a segmentace dimenze.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-153">Choose a visualization for your query and select the fields for x-axis, y-axis, and segmenting dimension.</span></span>
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. <span data-ttu-id="1c9d4-155">Publikujte sestavy do pracovního prostoru cloudu Power BI.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-155">Publish your report to your Power BI cloud workspace.</span></span> <span data-ttu-id="1c9d4-156">Odtud můžete synchronizované verze vložit do jiných webových stránek.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-156">From there, you can embed a synchronized version into other web pages.</span></span>
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/publish-power-bi.png)
4. <span data-ttu-id="1c9d4-158">Sestavu v intervalech aktualizovat ručně, nebo nastavit plánovaná aktualizace na stránce Možnosti.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-158">Refresh the report manually at intervals, or set up a scheduled refresh on the options page.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1c9d4-159">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="1c9d4-159">Troubleshooting</span></span>

### <a name="401-or-403-unauthorized"></a><span data-ttu-id="1c9d4-160">401 nebo 403 Neautorizováno</span><span class="sxs-lookup"><span data-stu-id="1c9d4-160">401 or 403 Unauthorized</span></span> 
<span data-ttu-id="1c9d4-161">To může dojít, pokud zatím není aktualizovaná refesh tokenu.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-161">This can happen if your refesh token has not been updated.</span></span> <span data-ttu-id="1c9d4-162">Opakujte tyto kroky, které zajišťují, že máte přístup.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-162">Try these steps to ensure you still have access.</span></span> <span data-ttu-id="1c9d4-163">Pokud máte přístup a refershing přihlašovací údaje se nedají použít, otevřete prosím lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-163">If you do have access and refershing the credentials does not work, please open a support ticket.</span></span>

1. <span data-ttu-id="1c9d4-164">Přihlaste se k portálu Azure a ujistěte se, že má přístup k prostředku</span><span class="sxs-lookup"><span data-stu-id="1c9d4-164">Log into the Azure Portal and make sure you can access the resource</span></span>
2. <span data-ttu-id="1c9d4-165">Zkuste aktualizovat přihlašovací údaje pro řídicí panel</span><span class="sxs-lookup"><span data-stu-id="1c9d4-165">Try to refresh the credentials for the Dashboard</span></span>

### <a name="502-bad-gateway"></a><span data-ttu-id="1c9d4-166">502 Chybná brána</span><span class="sxs-lookup"><span data-stu-id="1c9d4-166">502 Bad Gateway</span></span>
<span data-ttu-id="1c9d4-167">To je obvykle způsobeno dotazu analýzy, který vrátí příliš mnoho dat.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-167">This is usually caused by an Analytics query that returns too much data.</span></span> <span data-ttu-id="1c9d4-168">Měli zkuste použít menší časový rozsah nebo pomocí [před](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) nebo [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) funguje pouze [projektu](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) pole, je nutné.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-168">You should try using a smaller time range or by using the [ago](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) or [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) functions only [project](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) the fields you need.</span></span>

<span data-ttu-id="1c9d4-169">Pokud snižuje datovou sadu pocházejících z dotazu Analytics není splňují vaše požadavky měli byste zvážit použití [rozhraní API](https://dev.applicationinsights.io/documentation/overview) vyžádat větší datové sady.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-169">If reducing the dataset coming from the Analytics query doesn't meet your requirements you should consider using the [API](https://dev.applicationinsights.io/documentation/overview) to pull a larger dataset.</span></span> <span data-ttu-id="1c9d4-170">Tady jsou pokyny, jak převést export M dotazu použít rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-170">Here are instructions on how to convert the M-Query export to use the API.</span></span>

1. <span data-ttu-id="1c9d4-171">Vytvoření [klíč rozhraní API](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span><span class="sxs-lookup"><span data-stu-id="1c9d4-171">Create an [API Key](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span></span>
2. <span data-ttu-id="1c9d4-172">Aktualizovat Power BI M skript, který jste exportovali z Analytics nahrazením adresu URL ARM AI rozhraní API (viz následující příklad)</span><span class="sxs-lookup"><span data-stu-id="1c9d4-172">Update the Power BI M script that you exported from Analytics by replacing the ARM URL with AI API (see example below)</span></span>
   * <span data-ttu-id="1c9d4-173">Nahraďte **https://management.azure.com/subscriptions/...**</span><span class="sxs-lookup"><span data-stu-id="1c9d4-173">Replace **https://management.azure.com/subscriptions/...**</span></span>
   * <span data-ttu-id="1c9d4-174">s **https://api.applicationinsights.io/beta/apps/...**</span><span class="sxs-lookup"><span data-stu-id="1c9d4-174">with, **https://api.applicationinsights.io/beta/apps/...**</span></span>
3. <span data-ttu-id="1c9d4-175">Nakonec aktualizujte přihlašovací údaje k základní a používání vašeho klíče rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1c9d4-175">Finally, update credentials to basic, and use your API Key</span></span>
  

<span data-ttu-id="1c9d4-176">**Existující skriptu**</span><span class="sxs-lookup"><span data-stu-id="1c9d4-176">**Existing Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
<span data-ttu-id="1c9d4-177">**Aktualizované skriptu**</span><span class="sxs-lookup"><span data-stu-id="1c9d4-177">**Updated Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a><span data-ttu-id="1c9d4-178">O vzorkování</span><span class="sxs-lookup"><span data-stu-id="1c9d4-178">About sampling</span></span>
<span data-ttu-id="1c9d4-179">Pokud vaše aplikace odešle velké množství dat, může funkce adaptivního vzorkování pracovat a odesílat pouze procento vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-179">If your application sends a lot of data, the adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> <span data-ttu-id="1c9d4-180">Totéž platí, pokud jste ručně nastavili vzorkování buď v sadě SDK, nebo na přijímání.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-180">The same is true if you have manually set sampling either in the SDK or on ingestion.</span></span> [<span data-ttu-id="1c9d4-181">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="1c9d4-181">Learn more about sampling.</span></span>](app-insights-sampling.md)


## <a name="next-steps"></a><span data-ttu-id="1c9d4-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c9d4-182">Next steps</span></span>
* [<span data-ttu-id="1c9d4-183">Power BI - Další informace</span><span class="sxs-lookup"><span data-stu-id="1c9d4-183">Power BI - Learn</span></span>](http://www.powerbi.com/learning/)
* [<span data-ttu-id="1c9d4-184">Analýza kurzu</span><span class="sxs-lookup"><span data-stu-id="1c9d4-184">Analytics tutorial</span></span>](app-insights-analytics-tour.md)

