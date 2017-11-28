---
title: "aaaExport pomocí služby Stream Analytics z Azure Application Insights | Microsoft Docs"
description: "Stream Analytics můžete nepřetržitě transformace, filtr a hello data trasy, které exportujete z Application Insights."
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
ms.openlocfilehash: fda9b64f588c520833b2669eafdf650efc3de6be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-stream-analytics-tooprocess-exported-data-from-application-insights"></a><span data-ttu-id="e7f57-103">Použití Stream Analytics tooprocess exportovaná data ze služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="e7f57-103">Use Stream Analytics tooprocess exported data from Application Insights</span></span>
<span data-ttu-id="e7f57-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) je hello ideální nástroj pro zpracování dat [exportovaný z Application Insights](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="e7f57-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) is hello ideal tool for processing data [exported from Application Insights](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="e7f57-105">Stream Analytics můžete načítat data z různých zdrojů.</span><span class="sxs-lookup"><span data-stu-id="e7f57-105">Stream Analytics can pull data from a variety of sources.</span></span> <span data-ttu-id="e7f57-106">Můžete transformovat a filtrování dat hello a pak směruje tooa řadu jímky.</span><span class="sxs-lookup"><span data-stu-id="e7f57-106">It can transform and filter hello data, and then route it tooa variety of sinks.</span></span>

<span data-ttu-id="e7f57-107">V tomto příkladu vytvoříme adaptéru, která vezme všechna data ze služby Application Insights, přejmenuje a zpracovává některá pole hello a prostřednictvím kanálu předá ho do Power BI.</span><span class="sxs-lookup"><span data-stu-id="e7f57-107">In this example, we'll create an adaptor that takes data from Application Insights, renames and processes some of hello fields, and pipes it into Power BI.</span></span>

> [!WARNING]
> <span data-ttu-id="e7f57-108">Existují mnohem lepší a jednodušší [doporučené způsoby, jak toodisplay Application Insights data v Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="e7f57-108">There are much better and easier [recommended ways toodisplay Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="e7f57-109">Hello cesta zde znázorněný je právě tooillustrate příklad jak tooprocess exportovat data.</span><span class="sxs-lookup"><span data-stu-id="e7f57-109">hello path illustrated here is just an example tooillustrate how tooprocess exported data.</span></span>
> 
> 

![Blokový diagram pro export prostřednictvím SA tooPBI](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a><span data-ttu-id="e7f57-111">Vytvořte úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="e7f57-111">Create storage in Azure</span></span>
<span data-ttu-id="e7f57-112">Průběžné export vždy výstupy účet úložiště Azure tooan dat, proto musíte nejprve toocreate hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="e7f57-112">Continuous export always outputs data tooan Azure Storage account, so you need toocreate hello storage first.</span></span>

1. <span data-ttu-id="e7f57-113">Vytvořit účet úložiště "classic" v rámci vašeho předplatného v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e7f57-113">Create a "classic" storage account in your subscription in hello [Azure portal](https://portal.azure.com).</span></span>
   
   ![Na portálu Azure zvolte nový, Data, úložiště](./media/app-insights-export-stream-analytics/030.png)
2. <span data-ttu-id="e7f57-115">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="e7f57-115">Create a container</span></span>
   
    ![V novém úložišti hello vyberte kontejnery, klikněte na dlaždici hello kontejnery a potom přidat](./media/app-insights-export-stream-analytics/040.png)
3. <span data-ttu-id="e7f57-117">Zkopírovat přístupový klíč k úložišti hello</span><span class="sxs-lookup"><span data-stu-id="e7f57-117">Copy hello storage access key</span></span>
   
    <span data-ttu-id="e7f57-118">Budete je potřebovat brzy tooset až hello vstupní toohello stream analytics služby.</span><span class="sxs-lookup"><span data-stu-id="e7f57-118">You'll need it soon tooset up hello input toohello stream analytics service.</span></span>
   
    ![V úložišti hello otevře se nastavení klíče a proveďte kopii hello primární přístupový klíč](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-tooazure-storage"></a><span data-ttu-id="e7f57-120">Spustit průběžné export tooAzure úložiště</span><span class="sxs-lookup"><span data-stu-id="e7f57-120">Start continuous export tooAzure storage</span></span>
<span data-ttu-id="e7f57-121">[Průběžné export](app-insights-export-telemetry.md) přesune data z Application Insights do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e7f57-121">[Continuous export](app-insights-export-telemetry.md) moves data from Application Insights into Azure storage.</span></span>

1. <span data-ttu-id="e7f57-122">V hello portálu Azure vyhledejte prostředek Application Insights toohello, který jste vytvořili pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7f57-122">In hello Azure portal, browse toohello Application Insights resource you created for your application.</span></span>
   
    ![Vyberte možnost Procházet, Application Insights, vaše aplikace](./media/app-insights-export-stream-analytics/050.png)
2. <span data-ttu-id="e7f57-124">Vytvořte průběžné export.</span><span class="sxs-lookup"><span data-stu-id="e7f57-124">Create a continuous export.</span></span>
   
    ![Vyberte nastavení, průběžné exportu, přidejte](./media/app-insights-export-stream-analytics/060.png)

    <span data-ttu-id="e7f57-126">Vyberte účet úložiště hello, které jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="e7f57-126">Select hello storage account you created earlier:</span></span>

    ![Nastavit cíl exportu hello](./media/app-insights-export-stream-analytics/070.png)

    <span data-ttu-id="e7f57-128">Nastavte hello typy událostí, které se mají toosee:</span><span class="sxs-lookup"><span data-stu-id="e7f57-128">Set hello event types you want toosee:</span></span>

    ![Vyberte typy událostí](./media/app-insights-export-stream-analytics/080.png)

1. <span data-ttu-id="e7f57-130">Některá data hromadí let.</span><span class="sxs-lookup"><span data-stu-id="e7f57-130">Let some data accumulate.</span></span> <span data-ttu-id="e7f57-131">Sledujte a umožnit lidem nějakou dobu používat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7f57-131">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="e7f57-132">Telemetrická data se odešlou a zobrazí statistické grafy v [metriky explorer](app-insights-metrics-explorer.md) a jednotlivé události v [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="e7f57-132">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="e7f57-133">A navíc hello dat bude exportovat tooyour úložiště.</span><span class="sxs-lookup"><span data-stu-id="e7f57-133">And also, hello data will export tooyour storage.</span></span> 
2. <span data-ttu-id="e7f57-134">Zkontrolujte hello exportovat data.</span><span class="sxs-lookup"><span data-stu-id="e7f57-134">Inspect hello exported data.</span></span> <span data-ttu-id="e7f57-135">V sadě Visual Studio, vyberte **zobrazení / cloudu Explorer**a otevřete Azure nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="e7f57-135">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="e7f57-136">(Pokud nemáte tento příkaz nabídky, je třeba tooinstall hello Azure SDK: Otevřete dialogové okno Nový projekt hello a otevřete Visual C# / Cloud / získat Microsoft Azure SDK pro .NET.)</span><span class="sxs-lookup"><span data-stu-id="e7f57-136">(If you don't have this menu option, you need tooinstall hello Azure SDK: Open hello New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    <span data-ttu-id="e7f57-137">Poznamenejte si hello běžné součástí hello název cesty, které je odvozeno z názvu a instrumentace klíč aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-137">Make a note of hello common part of hello path name, which is derived from hello application name and instrumentation key.</span></span> 

<span data-ttu-id="e7f57-138">Hello události se zapisují tooblob soubory ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="e7f57-138">hello events are written tooblob files in JSON format.</span></span> <span data-ttu-id="e7f57-139">Každý soubor může obsahovat jeden nebo více událostí.</span><span class="sxs-lookup"><span data-stu-id="e7f57-139">Each file may contain one or more events.</span></span> <span data-ttu-id="e7f57-140">Proto rádi bychom znali data události hello tooread a filtrování hello pole, která má být.</span><span class="sxs-lookup"><span data-stu-id="e7f57-140">So we'd like tooread hello event data and filter out hello fields we want.</span></span> <span data-ttu-id="e7f57-141">Jsou k dispozici všechny typy věcí, které můžeme udělat s daty hello, ale naše plán dnes je toouse Stream Analytics toopipe hello data tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="e7f57-141">There are all kinds of things we could do with hello data, but our plan today is toouse Stream Analytics toopipe hello data tooPower BI.</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="e7f57-142">Vytvoření instance služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e7f57-142">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="e7f57-143">Z hello [portál Azure Classic](https://manage.windowsazure.com/), vyberte službu Azure Stream Analytics hello a vytvořit novou úlohu služby Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="e7f57-143">From hello [Classic Azure Portal](https://manage.windowsazure.com/), select hello Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

<span data-ttu-id="e7f57-144">Když je vytvořena nová úloha hello, rozbalte položku Podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="e7f57-144">When hello new job is created, expand its details:</span></span>

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a><span data-ttu-id="e7f57-145">Nastavení umístění objektu blob</span><span class="sxs-lookup"><span data-stu-id="e7f57-145">Set blob location</span></span>
<span data-ttu-id="e7f57-146">Nastavte tootake vstup z objektu blob služby průběžné Export:</span><span class="sxs-lookup"><span data-stu-id="e7f57-146">Set it tootake input from your Continuous Export blob:</span></span>

![](./media/app-insights-export-stream-analytics/120.png)

<span data-ttu-id="e7f57-147">Nyní budete potřebovat hello primární přístupový klíč z vašeho účtu úložiště, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="e7f57-147">Now you'll need hello Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="e7f57-148">Nastavením této hodnoty jako hello klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e7f57-148">Set this as hello Storage Account Key.</span></span>

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a><span data-ttu-id="e7f57-149">Sada cesta předpona vzoru</span><span class="sxs-lookup"><span data-stu-id="e7f57-149">Set path prefix pattern</span></span>
![](./media/app-insights-export-stream-analytics/140.png)

<span data-ttu-id="e7f57-150">**Být jisti tooset hello formát data tooYYYY-MM-DD (s pomlčkami).**</span><span class="sxs-lookup"><span data-stu-id="e7f57-150">**Be sure tooset hello Date Format tooYYYY-MM-DD (with dashes).**</span></span>

<span data-ttu-id="e7f57-151">Hello cesta předpony vzor Určuje, kde Stream Analytics vyhledá hello vstupní soubory v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-151">hello Path Prefix Pattern specifies where Stream Analytics finds hello input files in hello storage.</span></span> <span data-ttu-id="e7f57-152">Je třeba tooset ho toocorrespond toohow průběžné exportovat ukládá hello data.</span><span class="sxs-lookup"><span data-stu-id="e7f57-152">You need tooset it toocorrespond toohow Continuous Export stores hello data.</span></span> <span data-ttu-id="e7f57-153">Nastavte takto:</span><span class="sxs-lookup"><span data-stu-id="e7f57-153">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="e7f57-154">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="e7f57-154">In this example:</span></span>

* <span data-ttu-id="e7f57-155">`webapplication27`je název hello hello prostředek Application Insights **malá všechny**.</span><span class="sxs-lookup"><span data-stu-id="e7f57-155">`webapplication27` is hello name of hello Application Insights resource **all lower case**.</span></span>
* <span data-ttu-id="e7f57-156">`1234...`je klíč instrumentace hello hello prostředku Application Insights **vynechání pomlčky**.</span><span class="sxs-lookup"><span data-stu-id="e7f57-156">`1234...` is hello instrumentation key of hello Application Insights resource, **omitting dashes**.</span></span> 
* <span data-ttu-id="e7f57-157">`PageViews`je hello typ dat chcete tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="e7f57-157">`PageViews` is hello type of data you want tooanalyze.</span></span> <span data-ttu-id="e7f57-158">dostupné typy Hello závisí na hello filtr, který nastavíte v průběžné exportovat.</span><span class="sxs-lookup"><span data-stu-id="e7f57-158">hello available types depend on hello filter you set in Continuous Export.</span></span> <span data-ttu-id="e7f57-159">Zkontrolujte hello exportovaná data toosee hello jiné typy k dispozici a zobrazí hello [Exportovat datový model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="e7f57-159">Examine hello exported data toosee hello other available types, and see hello [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="e7f57-160">`/{date}/{time}`vzor zapsána oznámena.</span><span class="sxs-lookup"><span data-stu-id="e7f57-160">`/{date}/{time}` is a pattern written literally.</span></span>

> [!NOTE]
> <span data-ttu-id="e7f57-161">Zkontrolujte toomake úložiště hello se, že můžete získat cestu hello správné.</span><span class="sxs-lookup"><span data-stu-id="e7f57-161">Inspect hello storage toomake sure you get hello path right.</span></span>
> 
> 

### <a name="finish-initial-setup"></a><span data-ttu-id="e7f57-162">Dokončit počáteční nastavení</span><span class="sxs-lookup"><span data-stu-id="e7f57-162">Finish initial setup</span></span>
<span data-ttu-id="e7f57-163">Zkontrolujte formát serializace hello:</span><span class="sxs-lookup"><span data-stu-id="e7f57-163">Confirm hello serialization format:</span></span>

![Potvrďte a zavřete průvodce](./media/app-insights-export-stream-analytics/150.png)

<span data-ttu-id="e7f57-165">Zavřete průvodce hello a počkejte toocomplete hello instalační program.</span><span class="sxs-lookup"><span data-stu-id="e7f57-165">Close hello wizard and wait for hello setup toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="e7f57-166">Použijte hello Ukázkový příkaz toodownload některá data.</span><span class="sxs-lookup"><span data-stu-id="e7f57-166">Use hello Sample command toodownload some data.</span></span> <span data-ttu-id="e7f57-167">Umožňuje zachovat ji jako ukázka toodebug testovací dotazu.</span><span class="sxs-lookup"><span data-stu-id="e7f57-167">Keep it as a test sample toodebug your query.</span></span>
> 
> 

## <a name="set-hello-output"></a><span data-ttu-id="e7f57-168">Výstup hello sady</span><span class="sxs-lookup"><span data-stu-id="e7f57-168">Set hello output</span></span>
<span data-ttu-id="e7f57-169">Nyní vyberte úlohu a nastavte výstup hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-169">Now select your job and set hello output.</span></span>

![Vyberte nový kanál hello, klikněte na výstupy, přidat, Power BI](./media/app-insights-export-stream-analytics/160.png)

<span data-ttu-id="e7f57-171">Zadejte vaše **pracovní nebo školní účet** tooauthorize tooaccess Stream Analytics prostředku Power BI.</span><span class="sxs-lookup"><span data-stu-id="e7f57-171">Provide your **work or school account** tooauthorize Stream Analytics tooaccess your Power BI resource.</span></span> <span data-ttu-id="e7f57-172">Pak vytvořte název pro výstup hello a pro datovou sadu Power BI hello cíl a tabulku.</span><span class="sxs-lookup"><span data-stu-id="e7f57-172">Then invent a name for hello output, and for hello target Power BI dataset and table.</span></span>

![Skladová tři názvy](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-hello-query"></a><span data-ttu-id="e7f57-174">Sada hello dotazu</span><span class="sxs-lookup"><span data-stu-id="e7f57-174">Set hello query</span></span>
<span data-ttu-id="e7f57-175">dotaz Hello řídí hello překlad ze vstupní toooutput.</span><span class="sxs-lookup"><span data-stu-id="e7f57-175">hello query governs hello translation from input toooutput.</span></span>

![Vyberte hello úlohu a klikněte na dotaz.](./media/app-insights-export-stream-analytics/180.png)

<span data-ttu-id="e7f57-178">Použijte hello testovací funkce toocheck získat výstup hello správné.</span><span class="sxs-lookup"><span data-stu-id="e7f57-178">Use hello Test function toocheck that you get hello right output.</span></span> <span data-ttu-id="e7f57-179">Poskytněte hello ukázková data, která trvala ze stránky hello vstupy.</span><span class="sxs-lookup"><span data-stu-id="e7f57-179">Give it hello sample data that you took from hello inputs page.</span></span> 

### <a name="query-toodisplay-counts-of-events"></a><span data-ttu-id="e7f57-180">Dotaz toodisplay počet událostí</span><span class="sxs-lookup"><span data-stu-id="e7f57-180">Query toodisplay counts of events</span></span>
<span data-ttu-id="e7f57-181">Vložte tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="e7f57-181">Paste this query:</span></span>

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

* <span data-ttu-id="e7f57-182">Export vstup je alias hello jsme vám Dal toohello vstupní datový proud</span><span class="sxs-lookup"><span data-stu-id="e7f57-182">export-input is hello alias we gave toohello stream input</span></span>
* <span data-ttu-id="e7f57-183">alias pro výstup hello, které jsme definovali je pbi-output</span><span class="sxs-lookup"><span data-stu-id="e7f57-183">pbi-output is hello output alias we defined</span></span>
* <span data-ttu-id="e7f57-184">Používáme [vnější použít GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) vzhledem k tomu, že název události hello je ve vnořené arrray JSON.</span><span class="sxs-lookup"><span data-stu-id="e7f57-184">We use [OUTER APPLY GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) because hello event name is in a nested JSON arrray.</span></span> <span data-ttu-id="e7f57-185">Potom vyberte vyskladnění hello hello název události, spolu s počtem hello počet instancí s tímto názvem v hello časové období.</span><span class="sxs-lookup"><span data-stu-id="e7f57-185">Then hello Select picks hello event name, together with a count of hello number of instances with that name in hello time period.</span></span> <span data-ttu-id="e7f57-186">Hello [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) klauzule skupiny hello elementy do časových období 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="e7f57-186">hello [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) clause groups hello elements into time periods of 1 minute.</span></span>

### <a name="query-toodisplay-metric-values"></a><span data-ttu-id="e7f57-187">Hodnoty metriky toodisplay dotazu</span><span class="sxs-lookup"><span data-stu-id="e7f57-187">Query toodisplay metric values</span></span>
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

* <span data-ttu-id="e7f57-188">Tento dotaz prochází do hello metriky telemetrie tooget hello čas události a metriky hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-188">This query drills into hello metrics telemetry tooget hello event time and hello metric value.</span></span> <span data-ttu-id="e7f57-189">Hello metriky hodnoty jsou uvnitř pole, takže používáme hello vnější GetElements použít vzor tooextract hello řádků.</span><span class="sxs-lookup"><span data-stu-id="e7f57-189">hello metric values are inside an array, so we use hello OUTER APPLY GetElements pattern tooextract hello rows.</span></span> <span data-ttu-id="e7f57-190">"myMetric" v tomto případě je název hello metrika hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-190">"myMetric" is hello name of hello metric in this case.</span></span> 

### <a name="query-tooinclude-values-of-dimension-properties"></a><span data-ttu-id="e7f57-191">Dotaz tooinclude hodnoty vlastností dimenze</span><span class="sxs-lookup"><span data-stu-id="e7f57-191">Query tooinclude values of dimension properties</span></span>
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

* <span data-ttu-id="e7f57-192">Tento dotaz obsahuje hodnoty vlastností dimenze hello bez v závislosti na konkrétní dimenzi probíhá na pevnou index v poli dimenze hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-192">This query includes values of hello dimension properties without depending on a particular dimension being at a fixed index in hello dimension array.</span></span>

## <a name="run-hello-job"></a><span data-ttu-id="e7f57-193">Spustit úlohu hello</span><span class="sxs-lookup"><span data-stu-id="e7f57-193">Run hello job</span></span>
<span data-ttu-id="e7f57-194">Vyberte datum v hello po toostart hello úlohu.</span><span class="sxs-lookup"><span data-stu-id="e7f57-194">You can select a date in hello past toostart hello job from.</span></span> 

![Vyberte hello úlohu a klikněte na dotaz.](./media/app-insights-export-stream-analytics/190.png)

<span data-ttu-id="e7f57-197">Počkejte, dokud je spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-197">Wait until hello job is Running.</span></span>

## <a name="see-results-in-power-bi"></a><span data-ttu-id="e7f57-198">Zobrazte výsledky v Power BI</span><span class="sxs-lookup"><span data-stu-id="e7f57-198">See results in Power BI</span></span>
> [!WARNING]
> <span data-ttu-id="e7f57-199">Existují mnohem lepší a jednodušší [doporučené způsoby, jak toodisplay Application Insights data v Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="e7f57-199">There are much better and easier [recommended ways toodisplay Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="e7f57-200">Hello cesta zde znázorněný je právě tooillustrate příklad jak tooprocess exportovat data.</span><span class="sxs-lookup"><span data-stu-id="e7f57-200">hello path illustrated here is just an example tooillustrate how tooprocess exported data.</span></span>
> 
> 

<span data-ttu-id="e7f57-201">Otevřít Power BI s použitím pracovní nebo školní účet a vyberte hello datovou sadu a tabulky, který jste definovali jako výstup hello úlohy Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="e7f57-201">Open Power BI with your work or school account, and select hello dataset and table that you defined as hello output of hello Stream Analytics job.</span></span>

![V Power BI vyberte pole a datové sady.](./media/app-insights-export-stream-analytics/200.png)

<span data-ttu-id="e7f57-203">Teď můžete použít tuto datovou sadu v sestavy a řídicí panely v [Power BI](https://powerbi.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="e7f57-203">Now you can use this dataset in reports and dashboards in [Power BI](https://powerbi.microsoft.com).</span></span>

![V Power BI vyberte pole a datové sady.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a><span data-ttu-id="e7f57-205">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="e7f57-205">No data?</span></span>
* <span data-ttu-id="e7f57-206">Zkontrolujte, které [formátu datum hello sady](#set-path-prefix-pattern) správně tooYYYY-MM-DD (s pomlčkami).</span><span class="sxs-lookup"><span data-stu-id="e7f57-206">Check that you [set hello date format](#set-path-prefix-pattern) correctly tooYYYY-MM-DD (with dashes).</span></span>

## <a name="video"></a><span data-ttu-id="e7f57-207">Video</span><span class="sxs-lookup"><span data-stu-id="e7f57-207">Video</span></span>
<span data-ttu-id="e7f57-208">Noam Ben Zeev ukazuje, jak tooprocess export dat pomocí služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="e7f57-208">Noam Ben Zeev shows how tooprocess exported data using Stream Analytics.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a><span data-ttu-id="e7f57-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7f57-209">Next steps</span></span>
* [<span data-ttu-id="e7f57-210">Průběžný export</span><span class="sxs-lookup"><span data-stu-id="e7f57-210">Continuous export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="e7f57-211">Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="e7f57-211">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="e7f57-212">Application Insights</span><span class="sxs-lookup"><span data-stu-id="e7f57-212">Application Insights</span></span>](app-insights-overview.md)

