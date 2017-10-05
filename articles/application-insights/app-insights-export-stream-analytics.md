---
title: "Exportovat pomocí služby Stream Analytics z Azure Application Insights | Microsoft Docs"
description: "Stream Analytics můžete nepřetržitě transformace, filtrovat a směrování dat, které exportujete z Application Insights."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 6a84d8ff67c420ce712de905ab1172632502a863
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a><span data-ttu-id="7eac3-103">Použití Stream Analytics ke zpracování exportovaná data ze služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="7eac3-103">Use Stream Analytics to process exported data from Application Insights</span></span>
<span data-ttu-id="7eac3-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) je ideální nástroj pro zpracování dat [exportovaný z Application Insights](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="7eac3-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) is the ideal tool for processing data [exported from Application Insights](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="7eac3-105">Stream Analytics můžete načítat data z různých zdrojů.</span><span class="sxs-lookup"><span data-stu-id="7eac3-105">Stream Analytics can pull data from a variety of sources.</span></span> <span data-ttu-id="7eac3-106">Může transformace a filtrovat data a pak směrovat celou řadu jímky.</span><span class="sxs-lookup"><span data-stu-id="7eac3-106">It can transform and filter the data, and then route it to a variety of sinks.</span></span>

<span data-ttu-id="7eac3-107">V tomto příkladu vytvoříme adaptéru, která vezme všechna data ze služby Application Insights, přejmenuje a zpracovává některá pole a prostřednictvím kanálu předá ho do Power BI.</span><span class="sxs-lookup"><span data-stu-id="7eac3-107">In this example, we'll create an adaptor that takes data from Application Insights, renames and processes some of the fields, and pipes it into Power BI.</span></span>

> [!WARNING]
> <span data-ttu-id="7eac3-108">Existují mnohem lepší a jednodušší [doporučené způsoby, jak zobrazit data Application Insights v Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="7eac3-108">There are much better and easier [recommended ways to display Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="7eac3-109">Cesta zde znázorněný je jenom jako příklad k ukazují, jak zpracovat exportovaná data.</span><span class="sxs-lookup"><span data-stu-id="7eac3-109">The path illustrated here is just an example to illustrate how to process exported data.</span></span>
> 
> 

![Blokový diagram pro export prostřednictvím SA ke PBI](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a><span data-ttu-id="7eac3-111">Vytvořte úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="7eac3-111">Create storage in Azure</span></span>
<span data-ttu-id="7eac3-112">Průběžné export vždy poskytuje data pro účet úložiště Azure, musíte nejprve vytvořit úložiště.</span><span class="sxs-lookup"><span data-stu-id="7eac3-112">Continuous export always outputs data to an Azure Storage account, so you need to create the storage first.</span></span>

1. <span data-ttu-id="7eac3-113">Vytvořit účet úložiště "classic" v rámci vašeho předplatného v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7eac3-113">Create a "classic" storage account in your subscription in the [Azure portal](https://portal.azure.com).</span></span>
   
   ![Na portálu Azure zvolte nový, Data, úložiště](./media/app-insights-export-stream-analytics/030.png)
2. <span data-ttu-id="7eac3-115">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="7eac3-115">Create a container</span></span>
   
    ![V novém úložišti vyberte kontejnery, klikněte na dlaždici kontejnery a potom přidat](./media/app-insights-export-stream-analytics/040.png)
3. <span data-ttu-id="7eac3-117">Kopii přístupového klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="7eac3-117">Copy the storage access key</span></span>
   
    <span data-ttu-id="7eac3-118">Musíte ho brzy k nastavení vstup do služby stream analytics.</span><span class="sxs-lookup"><span data-stu-id="7eac3-118">You'll need it soon to set up the input to the stream analytics service.</span></span>
   
    ![V úložišti otevře se nastavení klíče a proveďte kopii primární přístupový klíč](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a><span data-ttu-id="7eac3-120">Spustit průběžné export do úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="7eac3-120">Start continuous export to Azure storage</span></span>
<span data-ttu-id="7eac3-121">[Průběžné export](app-insights-export-telemetry.md) přesune data z Application Insights do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7eac3-121">[Continuous export](app-insights-export-telemetry.md) moves data from Application Insights into Azure storage.</span></span>

1. <span data-ttu-id="7eac3-122">Na portálu Azure přejděte do prostředku Application Insights, kterou jste vytvořili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7eac3-122">In the Azure portal, browse to the Application Insights resource you created for your application.</span></span>
   
    ![Vyberte možnost Procházet, Application Insights, vaše aplikace](./media/app-insights-export-stream-analytics/050.png)
2. <span data-ttu-id="7eac3-124">Vytvořte průběžné export.</span><span class="sxs-lookup"><span data-stu-id="7eac3-124">Create a continuous export.</span></span>
   
    ![Vyberte nastavení, průběžné exportu, přidejte](./media/app-insights-export-stream-analytics/060.png)

    <span data-ttu-id="7eac3-126">Vyberte účet úložiště, které jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="7eac3-126">Select the storage account you created earlier:</span></span>

    ![Nastavit cíl exportu](./media/app-insights-export-stream-analytics/070.png)

    <span data-ttu-id="7eac3-128">Nastavte typy událostí, které chcete zobrazit:</span><span class="sxs-lookup"><span data-stu-id="7eac3-128">Set the event types you want to see:</span></span>

    ![Vyberte typy událostí](./media/app-insights-export-stream-analytics/080.png)

1. <span data-ttu-id="7eac3-130">Některá data hromadí let.</span><span class="sxs-lookup"><span data-stu-id="7eac3-130">Let some data accumulate.</span></span> <span data-ttu-id="7eac3-131">Sledujte a umožnit lidem nějakou dobu používat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7eac3-131">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="7eac3-132">Telemetrická data se odešlou a zobrazí statistické grafy v [metriky explorer](app-insights-metrics-explorer.md) a jednotlivé události v [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="7eac3-132">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="7eac3-133">A navíc bude exportovat data do úložiště.</span><span class="sxs-lookup"><span data-stu-id="7eac3-133">And also, the data will export to your storage.</span></span> 
2. <span data-ttu-id="7eac3-134">Zkontrolujte exportovaná data.</span><span class="sxs-lookup"><span data-stu-id="7eac3-134">Inspect the exported data.</span></span> <span data-ttu-id="7eac3-135">V sadě Visual Studio, vyberte **zobrazení / cloudu Explorer**a otevřete Azure nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="7eac3-135">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="7eac3-136">(Pokud nemáte tento příkaz nabídky, budete muset nainstalovat sadu Azure SDK: Otevřete dialogové okno Nový projekt a otevřete Visual C# / Cloud / získat Microsoft Azure SDK pro .NET.)</span><span class="sxs-lookup"><span data-stu-id="7eac3-136">(If you don't have this menu option, you need to install the Azure SDK: Open the New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    <span data-ttu-id="7eac3-137">Poznamenejte si část běžný název cesty, které je odvozeno z klíče název a instrumentace aplikací.</span><span class="sxs-lookup"><span data-stu-id="7eac3-137">Make a note of the common part of the path name, which is derived from the application name and instrumentation key.</span></span> 

<span data-ttu-id="7eac3-138">Události se zapisují do objektu blob soubory ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="7eac3-138">The events are written to blob files in JSON format.</span></span> <span data-ttu-id="7eac3-139">Každý soubor může obsahovat jeden nebo více událostí.</span><span class="sxs-lookup"><span data-stu-id="7eac3-139">Each file may contain one or more events.</span></span> <span data-ttu-id="7eac3-140">Proto jsme chtěli číst data události a filtrovat pole, která má být.</span><span class="sxs-lookup"><span data-stu-id="7eac3-140">So we'd like to read the event data and filter out the fields we want.</span></span> <span data-ttu-id="7eac3-141">Jsou k dispozici všechny typy věcí, které můžeme udělat s daty, ale naše plán dnes je použití Stream Analytics k předat data do Power BI.</span><span class="sxs-lookup"><span data-stu-id="7eac3-141">There are all kinds of things we could do with the data, but our plan today is to use Stream Analytics to pipe the data to Power BI.</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="7eac3-142">Vytvoření instance služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7eac3-142">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="7eac3-143">Z [portál Azure Classic](https://manage.windowsazure.com/), vyberte službu Azure Stream Analytics a vytvořit novou úlohu služby Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="7eac3-143">From the [Classic Azure Portal](https://manage.windowsazure.com/), select the Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

<span data-ttu-id="7eac3-144">Když je vytvořena nová úloha, rozbalte položku Podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="7eac3-144">When the new job is created, expand its details:</span></span>

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a><span data-ttu-id="7eac3-145">Nastavení umístění objektu blob</span><span class="sxs-lookup"><span data-stu-id="7eac3-145">Set blob location</span></span>
<span data-ttu-id="7eac3-146">Nastavte ji tak, aby vstupní z objektu blob služby průběžné Export:</span><span class="sxs-lookup"><span data-stu-id="7eac3-146">Set it to take input from your Continuous Export blob:</span></span>

![](./media/app-insights-export-stream-analytics/120.png)

<span data-ttu-id="7eac3-147">Nyní budete potřebovat primární přístupový klíč z vašeho účtu úložiště, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="7eac3-147">Now you'll need the Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="7eac3-148">Nastavením této hodnoty jako klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7eac3-148">Set this as the Storage Account Key.</span></span>

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a><span data-ttu-id="7eac3-149">Sada cesta předpona vzoru</span><span class="sxs-lookup"><span data-stu-id="7eac3-149">Set path prefix pattern</span></span>
![](./media/app-insights-export-stream-analytics/140.png)

<span data-ttu-id="7eac3-150">**Nezapomeňte nastavit formát data do rrrr-MM-DD (s pomlčkami).**</span><span class="sxs-lookup"><span data-stu-id="7eac3-150">**Be sure to set the Date Format to YYYY-MM-DD (with dashes).**</span></span>

<span data-ttu-id="7eac3-151">Vzor předpony cesta Určuje, kde Stream Analytics vyhledá vstupní soubory v úložišti.</span><span class="sxs-lookup"><span data-stu-id="7eac3-151">The Path Prefix Pattern specifies where Stream Analytics finds the input files in the storage.</span></span> <span data-ttu-id="7eac3-152">Budete muset nastavit tak, aby odpovídaly jak průběžné exportovat data uloží.</span><span class="sxs-lookup"><span data-stu-id="7eac3-152">You need to set it to correspond to how Continuous Export stores the data.</span></span> <span data-ttu-id="7eac3-153">Nastavte takto:</span><span class="sxs-lookup"><span data-stu-id="7eac3-153">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="7eac3-154">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="7eac3-154">In this example:</span></span>

* <span data-ttu-id="7eac3-155">`webapplication27`je název prostředku Application Insights **malá všechny**.</span><span class="sxs-lookup"><span data-stu-id="7eac3-155">`webapplication27` is the name of the Application Insights resource **all lower case**.</span></span>
* <span data-ttu-id="7eac3-156">`1234...`je klíč instrumentace prostředku Application Insights **vynechání pomlčky**.</span><span class="sxs-lookup"><span data-stu-id="7eac3-156">`1234...` is the instrumentation key of the Application Insights resource, **omitting dashes**.</span></span> 
* <span data-ttu-id="7eac3-157">`PageViews`je typu dat, který chcete analyzovat.</span><span class="sxs-lookup"><span data-stu-id="7eac3-157">`PageViews` is the type of data you want to analyze.</span></span> <span data-ttu-id="7eac3-158">Dostupné typy závisí na filtr, který nastavíte v průběžné exportovat.</span><span class="sxs-lookup"><span data-stu-id="7eac3-158">The available types depend on the filter you set in Continuous Export.</span></span> <span data-ttu-id="7eac3-159">Zkontrolujte exportovaná data zobrazit dostupné typy a zobrazit [Exportovat datový model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="7eac3-159">Examine the exported data to see the other available types, and see the [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="7eac3-160">`/{date}/{time}`vzor zapsána oznámena.</span><span class="sxs-lookup"><span data-stu-id="7eac3-160">`/{date}/{time}` is a pattern written literally.</span></span>

> [!NOTE]
> <span data-ttu-id="7eac3-161">Zkontrolujte úložiště a ujistěte se, že správné získání cesty.</span><span class="sxs-lookup"><span data-stu-id="7eac3-161">Inspect the storage to make sure you get the path right.</span></span>
> 
> 

### <a name="finish-initial-setup"></a><span data-ttu-id="7eac3-162">Dokončit počáteční nastavení</span><span class="sxs-lookup"><span data-stu-id="7eac3-162">Finish initial setup</span></span>
<span data-ttu-id="7eac3-163">Zkontrolujte formát serializace:</span><span class="sxs-lookup"><span data-stu-id="7eac3-163">Confirm the serialization format:</span></span>

![Potvrďte a zavřete průvodce](./media/app-insights-export-stream-analytics/150.png)

<span data-ttu-id="7eac3-165">Zavřete průvodce a počkejte na dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="7eac3-165">Close the wizard and wait for the setup to complete.</span></span>

> [!TIP]
> <span data-ttu-id="7eac3-166">Pomocí příkazu ukázka stáhnout některá data.</span><span class="sxs-lookup"><span data-stu-id="7eac3-166">Use the Sample command to download some data.</span></span> <span data-ttu-id="7eac3-167">Zachovat ji jako ukázka testu k ladění dotazu.</span><span class="sxs-lookup"><span data-stu-id="7eac3-167">Keep it as a test sample to debug your query.</span></span>
> 
> 

## <a name="set-the-output"></a><span data-ttu-id="7eac3-168">Nastavte výstup</span><span class="sxs-lookup"><span data-stu-id="7eac3-168">Set the output</span></span>
<span data-ttu-id="7eac3-169">Nyní vyberte úlohu a nastavte výstup.</span><span class="sxs-lookup"><span data-stu-id="7eac3-169">Now select your job and set the output.</span></span>

![Vyberte nový kanál, klikněte na výstupy, přidat, Power BI](./media/app-insights-export-stream-analytics/160.png)

<span data-ttu-id="7eac3-171">Zadejte vaše **pracovní nebo školní účet** k autorizaci Stream Analytics pro přístup k prostředku Power BI.</span><span class="sxs-lookup"><span data-stu-id="7eac3-171">Provide your **work or school account** to authorize Stream Analytics to access your Power BI resource.</span></span> <span data-ttu-id="7eac3-172">Pak vytvořte název pro výstup a pro datovou sadu cíl Power BI a tabulku.</span><span class="sxs-lookup"><span data-stu-id="7eac3-172">Then invent a name for the output, and for the target Power BI dataset and table.</span></span>

![Skladová tři názvy](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-the-query"></a><span data-ttu-id="7eac3-174">Nastavení dotazu</span><span class="sxs-lookup"><span data-stu-id="7eac3-174">Set the query</span></span>
<span data-ttu-id="7eac3-175">Dotaz řídí překlad ze vstupu na výstup.</span><span class="sxs-lookup"><span data-stu-id="7eac3-175">The query governs the translation from input to output.</span></span>

![Vyberte úlohu a klikněte na dotaz.](./media/app-insights-export-stream-analytics/180.png)

<span data-ttu-id="7eac3-178">Zkontrolujte, že dostanete správné výstup, použijte funkci Test.</span><span class="sxs-lookup"><span data-stu-id="7eac3-178">Use the Test function to check that you get the right output.</span></span> <span data-ttu-id="7eac3-179">Poskytněte ukázková data, která trvala ze stránky vstupy.</span><span class="sxs-lookup"><span data-stu-id="7eac3-179">Give it the sample data that you took from the inputs page.</span></span> 

### <a name="query-to-display-counts-of-events"></a><span data-ttu-id="7eac3-180">Spočítá počet dotazů k zobrazení událostí</span><span class="sxs-lookup"><span data-stu-id="7eac3-180">Query to display counts of events</span></span>
<span data-ttu-id="7eac3-181">Vložte tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="7eac3-181">Paste this query:</span></span>

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* <span data-ttu-id="7eac3-182">Export vstup je alias, který jsme uvedl do datového proudu vstup</span><span class="sxs-lookup"><span data-stu-id="7eac3-182">export-input is the alias we gave to the stream input</span></span>
* <span data-ttu-id="7eac3-183">pbi-output se výstup alias, který jsme definovali</span><span class="sxs-lookup"><span data-stu-id="7eac3-183">pbi-output is the output alias we defined</span></span>
* <span data-ttu-id="7eac3-184">Používáme [vnější použít GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) vzhledem k tomu, že je z názvu události ve vnořených arrray JSON.</span><span class="sxs-lookup"><span data-stu-id="7eac3-184">We use [OUTER APPLY GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) because the event name is in a nested JSON arrray.</span></span> <span data-ttu-id="7eac3-185">Potom Select vybere název události, společně s počet instancí s tímto názvem v časovém období.</span><span class="sxs-lookup"><span data-stu-id="7eac3-185">Then the Select picks the event name, together with a count of the number of instances with that name in the time period.</span></span> <span data-ttu-id="7eac3-186">[Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) klauzule skupiny elementy do časových období 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="7eac3-186">The [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) clause groups the elements into time periods of 1 minute.</span></span>

### <a name="query-to-display-metric-values"></a><span data-ttu-id="7eac3-187">Dotaz k zobrazení hodnot metriky</span><span class="sxs-lookup"><span data-stu-id="7eac3-187">Query to display metric values</span></span>
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* <span data-ttu-id="7eac3-188">Tento dotaz prochází do telemetrie metriky získat čas události a metriky hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7eac3-188">This query drills into the metrics telemetry to get the event time and the metric value.</span></span> <span data-ttu-id="7eac3-189">Metriky hodnoty jsou uvnitř pole, takže jsme použít vzor vnější GetElements použít k extrahování řádky.</span><span class="sxs-lookup"><span data-stu-id="7eac3-189">The metric values are inside an array, so we use the OUTER APPLY GetElements pattern to extract the rows.</span></span> <span data-ttu-id="7eac3-190">"myMetric" v tomto případě je název metriky.</span><span class="sxs-lookup"><span data-stu-id="7eac3-190">"myMetric" is the name of the metric in this case.</span></span> 

### <a name="query-to-include-values-of-dimension-properties"></a><span data-ttu-id="7eac3-191">Dotaz pro zahrnout hodnoty vlastností dimenze</span><span class="sxs-lookup"><span data-stu-id="7eac3-191">Query to include values of dimension properties</span></span>
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* <span data-ttu-id="7eac3-192">Tento dotaz obsahuje hodnoty vlastností dimenze bez v závislosti na konkrétní dimenzi probíhá na pevnou index v poli dimenze.</span><span class="sxs-lookup"><span data-stu-id="7eac3-192">This query includes values of the dimension properties without depending on a particular dimension being at a fixed index in the dimension array.</span></span>

## <a name="run-the-job"></a><span data-ttu-id="7eac3-193">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="7eac3-193">Run the job</span></span>
<span data-ttu-id="7eac3-194">Vybrat datum v minulosti při spuštění úlohy z.</span><span class="sxs-lookup"><span data-stu-id="7eac3-194">You can select a date in the past to start the job from.</span></span> 

![Vyberte úlohu a klikněte na dotaz.](./media/app-insights-export-stream-analytics/190.png)

<span data-ttu-id="7eac3-197">Počkejte, dokud je úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="7eac3-197">Wait until the job is Running.</span></span>

## <a name="see-results-in-power-bi"></a><span data-ttu-id="7eac3-198">Zobrazte výsledky v Power BI</span><span class="sxs-lookup"><span data-stu-id="7eac3-198">See results in Power BI</span></span>
> [!WARNING]
> <span data-ttu-id="7eac3-199">Existují mnohem lepší a jednodušší [doporučené způsoby, jak zobrazit data Application Insights v Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="7eac3-199">There are much better and easier [recommended ways to display Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="7eac3-200">Cesta zde znázorněný je jenom jako příklad k ukazují, jak zpracovat exportovaná data.</span><span class="sxs-lookup"><span data-stu-id="7eac3-200">The path illustrated here is just an example to illustrate how to process exported data.</span></span>
> 
> 

<span data-ttu-id="7eac3-201">Otevřít Power BI se váš pracovní nebo školní účet a vyberte datovou sadu a tabulky, který jste definovali jako výstup úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="7eac3-201">Open Power BI with your work or school account, and select the dataset and table that you defined as the output of the Stream Analytics job.</span></span>

![V Power BI vyberte pole a datové sady.](./media/app-insights-export-stream-analytics/200.png)

<span data-ttu-id="7eac3-203">Teď můžete použít tuto datovou sadu v sestavy a řídicí panely v [Power BI](https://powerbi.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="7eac3-203">Now you can use this dataset in reports and dashboards in [Power BI](https://powerbi.microsoft.com).</span></span>

![V Power BI vyberte pole a datové sady.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a><span data-ttu-id="7eac3-205">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="7eac3-205">No data?</span></span>
* <span data-ttu-id="7eac3-206">Zkontrolujte, které [nastaven formát data](#set-path-prefix-pattern) správně na rrrr-MM-DD (s pomlčkami).</span><span class="sxs-lookup"><span data-stu-id="7eac3-206">Check that you [set the date format](#set-path-prefix-pattern) correctly to YYYY-MM-DD (with dashes).</span></span>

## <a name="video"></a><span data-ttu-id="7eac3-207">Video</span><span class="sxs-lookup"><span data-stu-id="7eac3-207">Video</span></span>
<span data-ttu-id="7eac3-208">Noam Ben Zeev ukazuje, jak zpracovat exportovaná data pomocí služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="7eac3-208">Noam Ben Zeev shows how to process exported data using Stream Analytics.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a><span data-ttu-id="7eac3-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7eac3-209">Next steps</span></span>
* [<span data-ttu-id="7eac3-210">Průběžný export</span><span class="sxs-lookup"><span data-stu-id="7eac3-210">Continuous export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="7eac3-211">Podrobný datový model referenční informace pro vlastnost typů a hodnot.</span><span class="sxs-lookup"><span data-stu-id="7eac3-211">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="7eac3-212">Application Insights</span><span class="sxs-lookup"><span data-stu-id="7eac3-212">Application Insights</span></span>](app-insights-overview.md)

