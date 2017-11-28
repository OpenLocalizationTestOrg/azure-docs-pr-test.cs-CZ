---
title: "vzorkování aaaTelemetry ve službě Azure Application Insights | Microsoft Docs"
description: Jak tookeep hello svazku telemetrie pod kontrolou.
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bwren
ms.openlocfilehash: e19c350d0a5f16736c100322a9922fcfbf5d7b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sampling-in-application-insights"></a><span data-ttu-id="ddc0b-103">Vzorkování ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="ddc0b-103">Sampling in Application Insights</span></span>


<span data-ttu-id="ddc0b-104">Vzorkování je funkce v [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ddc0b-104">Sampling is a feature in [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="ddc0b-105">Je, že hello doporučené způsob tooreduce telemetrie provozu a úložiště, při zachování statisticky správné analýzy dat aplikací.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-105">It is hello recommended way tooreduce telemetry traffic and storage, while preserving  a statistically correct analysis of application data.</span></span> <span data-ttu-id="ddc0b-106">Filtr Hello vybere položky, které se vztahují, tak, aby můžete procházet mezi položkami při provádění diagnostických šetření.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-106">hello filter selects items that are related, so that you can navigate between items when you are doing diagnostic investigations.</span></span>
<span data-ttu-id="ddc0b-107">Když metriky počty uvádíme tooyou hello portálu, jsou renormalized tootake účet hello vzorkování, toominimize žádný vliv na hello statistiky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-107">When metric counts are presented tooyou in hello portal, they are renormalized tootake account of hello sampling, toominimize any effect on hello statistics.</span></span>

<span data-ttu-id="ddc0b-108">Vzorkování snižuje náklady na provoz a data a umožňuje vyhnout se omezení.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-108">Sampling reduces traffic and data costs, and helps you avoid throttling.</span></span>

## <a name="in-brief"></a><span data-ttu-id="ddc0b-109">Stručný postup:</span><span class="sxs-lookup"><span data-stu-id="ddc0b-109">In brief:</span></span>
* <span data-ttu-id="ddc0b-110">Vzorkování uchovává 1 v  *n*  zaznamenává a zahodí hello rest.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-110">Sampling retains 1 in *n* records and discards hello rest.</span></span> <span data-ttu-id="ddc0b-111">Například je může zachovat události 1 v 5, vzorkovací frekvenci 20 %.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-111">For example, it might retain 1 in 5 events, a sampling rate of 20%.</span></span> 
* <span data-ttu-id="ddc0b-112">Vzorkování se stane automaticky, když vaše aplikace odešle velké množství telemetrických dat, v aplikacích pro ASP.NET web server.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-112">Sampling happens automatically if your application sends a lot of telemetry, in ASP.NET web server apps.</span></span>
* <span data-ttu-id="ddc0b-113">Můžete také nastavit vzorkování ručně, buď v hello portál hello ceny stránky; nebo v hello sady SDK technologie ASP.NET v souboru .config hello, snižte tooalso hello síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-113">You can also set sampling manually, either in hello portal on hello pricing page; or in hello ASP.NET SDK in hello .config file, tooalso reduce hello network traffic.</span></span>
* <span data-ttu-id="ddc0b-114">Pokud protokolu vlastní události a chcete toomake se, že sadu událostí, které je buď uchovávají nebo zrušených společně, ujistěte se, že budou mít stejnou hodnotu OperationId hello.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-114">If you log custom events and you want toomake sure that a set of events is either retained or discarded together, make sure that they have hello same OperationId value.</span></span>
* <span data-ttu-id="ddc0b-115">vzorkování Hello dělitel  *n*  údajně všechny záznamy v hello vlastnost `itemCount`, která v hledání se zobrazí pod hello popisný název "počtu žádostí o" nebo "počet událostí".</span><span class="sxs-lookup"><span data-stu-id="ddc0b-115">hello sampling divisor *n* is reported in each record in hello property `itemCount`, which in Search appears under hello friendly name "request count" or "event count".</span></span> <span data-ttu-id="ddc0b-116">Pokud vzorkování není v provozu se `itemCount==1`.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-116">When sampling is not in operation, `itemCount==1`.</span></span>
* <span data-ttu-id="ddc0b-117">Pokud píšete analytické dotazy, měli byste [vzít v úvahu vzorkování](app-insights-analytics-tour.md#counting-sampled-data).</span><span class="sxs-lookup"><span data-stu-id="ddc0b-117">If you write Analytics queries, you should [take account of sampling](app-insights-analytics-tour.md#counting-sampled-data).</span></span> <span data-ttu-id="ddc0b-118">Konkrétně místo jednoduše počítání záznamy, měli byste použít `summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-118">In particular, instead of simply counting records, you should use `summarize sum(itemCount)`.</span></span>

## <a name="types-of-sampling"></a><span data-ttu-id="ddc0b-119">Typy vzorkování</span><span class="sxs-lookup"><span data-stu-id="ddc0b-119">Types of sampling</span></span>
<span data-ttu-id="ddc0b-120">Existují tři metody vzorkování alternativní:</span><span class="sxs-lookup"><span data-stu-id="ddc0b-120">There are three alternative sampling methods:</span></span>

* <span data-ttu-id="ddc0b-121">**Adaptivního vzorkování** automaticky přizpůsobí hello objem telemetrická data odesílaná z hello SDK v aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-121">**Adaptive sampling** automatically adjusts hello volume of telemetry sent from hello SDK in your ASP.NET app.</span></span> <span data-ttu-id="ddc0b-122">Výchozí hodnota je ze sady SDK v 2.0.0-beta3.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-122">Default from SDK v 2.0.0-beta3.</span></span> <span data-ttu-id="ddc0b-123">Aktuálně dostupné pro ASP.NET serverové pouze telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-123">Currently available for ASP.NET server-side telemetry only.</span></span> 
* <span data-ttu-id="ddc0b-124">**Míry vzorkování** snižuje objem hello telemetrická data odesílaná ze serveru ASP.NET a z prohlížečů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-124">**Fixed-rate sampling** reduces hello volume of telemetry sent from both your ASP.NET server and from your users' browsers.</span></span> <span data-ttu-id="ddc0b-125">Můžete nastavit rychlost hello.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-125">You set hello rate.</span></span> <span data-ttu-id="ddc0b-126">Hello klient a server bude synchronizovat jejich vzorkování tak, že v hledání, mohou procházet mezi zobrazení související stránky a požadavky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-126">hello client and server will synchronize their sampling so that, in Search, you can navigate between related page views and requests.</span></span>
* <span data-ttu-id="ddc0b-127">**Přijímání vzorkování** funguje v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-127">**Ingestion sampling** works in hello Azure portal.</span></span> <span data-ttu-id="ddc0b-128">Zahodí některé hello telemetrická data přenášená z vaší aplikace, který nastavíte.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-128">It discards some of hello telemetry that arrives from your app, at a rate that you set.</span></span> <span data-ttu-id="ddc0b-129">Není omezit přenos telemetrie, ale umožňuje udržovat v rámci měsíční kvóta.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-129">It doesn't reduce telemetry traffic, but helps you keep within your monthly quota.</span></span> <span data-ttu-id="ddc0b-130">Hello velkých výhod přijímání vzorkování je, že můžete ho nastavit bez opětovného nasazení aplikace a funguje jednotně pro všechny servery a klienty.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-130">hello big advantage of ingestion sampling is that you can set it without redeploying your app, and it works uniformly for all servers and clients.</span></span> 

<span data-ttu-id="ddc0b-131">Pokud Adaptivní nebo pevné míry vzorkování v operaci, přijímání vzorkování je zakázána.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-131">If Adaptive or Fixed rate sampling are in operation, Ingestion sampling is disabled.</span></span>

## <a name="ingestion-sampling"></a><span data-ttu-id="ddc0b-132">Přijímání vzorkování</span><span class="sxs-lookup"><span data-stu-id="ddc0b-132">Ingestion sampling</span></span>
<span data-ttu-id="ddc0b-133">Tato forma vzorkování funguje v okamžiku hello kde hello telemetrie z vaší webový server, prohlížečů a zařízení dosáhne koncového bodu služby Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-133">This form of sampling operates at hello point where hello telemetry from your web server, browsers, and devices reaches hello Application Insights service endpoint.</span></span> <span data-ttu-id="ddc0b-134">I když nesnižuje přenosy hello telemetrie z vaší aplikace, je zkrátit dobu, kterou hello zpracovat a zachovává (a účtovat poplatek za) pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-134">Although it doesn't reduce hello telemetry traffic sent from your app, it does reduce hello amount processed and retained (and charged for) by Application Insights.</span></span>

<span data-ttu-id="ddc0b-135">Tento typ vzorkování použijte, pokud vaše aplikace často prochází přes jeho měsíční kvóta a nemáte možnost hello některou z hello typy vzorkování na základě sady SDK.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-135">Use this type of sampling if your app often goes over its monthly quota and you don't have hello option of using either of hello SDK-based types of sampling.</span></span> 

<span data-ttu-id="ddc0b-136">Nastavit hello vzorkovací frekvenci v hello kvóty a cena okno:</span><span class="sxs-lookup"><span data-stu-id="ddc0b-136">Set hello sampling rate in hello Quotas and Pricing blade:</span></span>

![V okně Přehled aplikace hello klikněte na tlačítko Nastavení, kvóty a ukázky, pak vyberte vzorkovací frekvenci a kliknutím na tlačítko Aktualizovat.](./media/app-insights-sampling/04.png)

<span data-ttu-id="ddc0b-138">Podobně jako ostatní typy vzorkování zachová hello algoritmus telemetrii související položky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-138">Like other types of sampling, hello algorithm retains related telemetry items.</span></span> <span data-ttu-id="ddc0b-139">Například pokud jste probíhá kontrola hello telemetrie ve vyhledávání, bude možné žádost hello toofind související tooa určité výjimky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-139">For example, when you're inspecting hello telemetry in Search, you'll be able toofind hello request related tooa particular exception.</span></span> <span data-ttu-id="ddc0b-140">Metrika počítá jako je například rychlost požadavků a rychlost výjimka správně zachovány.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-140">Metric counts such as request rate and exception rate are correctly retained.</span></span>

<span data-ttu-id="ddc0b-141">Datové body, které jsou zrušených vzorkování nejsou k dispozici ve všech funkcí, Application Insights, jako [průběžné exportovat](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="ddc0b-141">Data points that are discarded by sampling are not available in any Application Insights feature such as [Continuous Export](app-insights-export-telemetry.md).</span></span>

<span data-ttu-id="ddc0b-142">Přijímání vzorkování nepracuje při vzorkování Adaptivní nebo pevnou sazbou na základě sady SDK je v provozu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-142">Ingestion sampling doesn't operate while SDK-based adaptive or fixed-rate sampling is in operation.</span></span> <span data-ttu-id="ddc0b-143">Pokud hello vzorkovací frekvenci na hello SDK je menší než 100 %, pak hello přijímání vzorkovací frekvenci, který nastavíte ignorována.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-143">If hello sampling rate at hello SDK is less than 100%, then hello ingestion sampling rate that you set is ignored.</span></span>

> [!WARNING]
> <span data-ttu-id="ddc0b-144">Hello hodnoty zobrazené na dlaždici hello označuje hello hodnotu, která nastavíte pro přijímání vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-144">hello value shown on hello tile indicates hello value that you set for ingestion sampling.</span></span> <span data-ttu-id="ddc0b-145">Hello skutečné vzorkovací frekvenci nepředstavuje Pokud vzorkování SDK je v provozu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-145">It doesn't represent hello actual sampling rate if SDK sampling is in operation.</span></span>
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a><span data-ttu-id="ddc0b-146">Adaptivního vzorkování na webovém serveru</span><span class="sxs-lookup"><span data-stu-id="ddc0b-146">Adaptive sampling at your web server</span></span>
<span data-ttu-id="ddc0b-147">Adaptivního vzorkování je k dispozici pro hello Application Insights SDK pro aplikace ASP.NET v 2.0.0-beta3 nebo novější a je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-147">Adaptive sampling is available for hello Application Insights SDK for ASP.NET v 2.0.0-beta3 and later, and is enabled by default.</span></span> 

<span data-ttu-id="ddc0b-148">Adaptivního vzorkování ovlivňuje hello objem telemetrická data odesílaná ze serveru vaší webové aplikace toohello služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-148">Adaptive sampling affects hello volume of telemetry sent from your web server app toohello Application Insights service.</span></span> <span data-ttu-id="ddc0b-149">Hello svazku se upraví automaticky tookeep v rámci zadaný maximální rychlost přenosu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-149">hello volume is adjusted automatically tookeep within a specified maximum rate of traffic.</span></span>

<span data-ttu-id="ddc0b-150">Ho nepracuje na nízkou svazky telemetrie, takže aplikace při ladění nebo nebude mít vliv na web s nízkou využití.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-150">It doesn't operate at low volumes of telemetry, so an app in debugging or a website with low usage won't be affected.</span></span>

<span data-ttu-id="ddc0b-151">tooachieve hello cílový svazek, některé hello telemetrii vygenerovanou se zahodí.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-151">tooachieve hello target volume, some of hello telemetry generated is discarded.</span></span> <span data-ttu-id="ddc0b-152">Ale stejně jako jiné typy vzorkování hello algoritmus zachová související telemetrii položky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-152">But like other types of sampling, hello algorithm retains related telemetry items.</span></span> <span data-ttu-id="ddc0b-153">Například pokud jste probíhá kontrola hello telemetrie ve vyhledávání, bude možné žádost hello toofind související tooa určité výjimky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-153">For example, when you're inspecting hello telemetry in Search, you'll be able toofind hello request related tooa particular exception.</span></span> 

<span data-ttu-id="ddc0b-154">Metrika spočítá, jako jsou frekvence výjimky a rychlost požadavků se upravenou toocompensate pro hello vzorkování míru, tak, aby se v Průzkumníku metrika zobrazit přibližně správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-154">Metric counts such as request rate and exception rate are adjusted toocompensate for hello sampling rate, so that they show approximately correct values in Metric Explorer.</span></span>

<span data-ttu-id="ddc0b-155">**Aktualizace vašeho projektu NuGet** nejnovější balíčky toohello *předběžné verze* verzi Application Insights: hello projekt v Průzkumníku řešení klikněte pravým tlačítkem, zvolte spravovat balíčky NuGet, zkontrolujte **zahrnout předběžné verze** a vyhledejte Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-155">**Update your project's NuGet** packages toohello latest *pre-release* version of Application Insights: Right-click hello project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 

<span data-ttu-id="ddc0b-156">V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), můžete upravit několik parametrů v hello `AdaptiveSamplingTelemetryProcessor` uzlu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-156">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), you can adjust several parameters in hello `AdaptiveSamplingTelemetryProcessor` node.</span></span> <span data-ttu-id="ddc0b-157">Hello obrázky, zobrazí se výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="ddc0b-157">hello figures shown are hello default values:</span></span>

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    <span data-ttu-id="ddc0b-158">Hello cíl rychlost, kterou hello adaptivní algoritmus cílem je pro **na každém hostiteli serveru**.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-158">hello target rate that hello adaptive algorithm aims for **on each server host**.</span></span> <span data-ttu-id="ddc0b-159">Pokud vaše webová aplikace běží na mnoho hostitelů, snížit tak jako tooremain v rámci vaší cílový počet přenosů dat na portál Application Insights hello tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-159">If your web app runs on many hosts, reduce this value so as tooremain within your target rate of traffic at hello Application Insights portal.</span></span>
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    <span data-ttu-id="ddc0b-160">Hello interval, ve které hello se znovu zhodnotí aktuální rychlost telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-160">hello interval at which hello current rate of telemetry is re-evaluated.</span></span> <span data-ttu-id="ddc0b-161">Hodnocení je provést, protože klouzavého průměru.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-161">Evaluation is performed as a moving average.</span></span> <span data-ttu-id="ddc0b-162">Tooshorten může použít tento interval, pokud je vaše telemetrie zodpovědná toosudden shluky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-162">You might want tooshorten this interval if your telemetry is liable toosudden bursts.</span></span>
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    <span data-ttu-id="ddc0b-163">V případě změny hodnot procento vzorkování, jak brzy po jsou jsme povoleno toolower vzorkování procento znovu toocapture méně data.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-163">When sampling percentage value changes, how soon after are we allowed toolower sampling percentage again toocapture less data.</span></span>
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    <span data-ttu-id="ddc0b-164">V případě změny hodnot procento vzorkování, jak brzy po jsou jsme povoleno tooincrease vzorkování procento znovu toocapture další data.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-164">When sampling percentage value changes, how soon after are we allowed tooincrease sampling percentage again toocapture more data.</span></span>
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    <span data-ttu-id="ddc0b-165">Protože vzorkování procento liší, jaká je minimální hodnota hello tooset jsme máte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-165">As sampling percentage varies, what is hello minimum value we're allowed tooset.</span></span>
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    <span data-ttu-id="ddc0b-166">Protože vzorkování procento liší, jaká je maximální hodnota hello tooset jsme máte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-166">As sampling percentage varies, what is hello maximum value we're allowed tooset.</span></span>
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    <span data-ttu-id="ddc0b-167">V hello výpočet hello klouzavý průměr přiřazena hello váhy toohello poslední hodnota.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-167">In hello calculation of hello moving average, hello weight assigned toohello most recent value.</span></span> <span data-ttu-id="ddc0b-168">Použijte stejné tooor hodnotu menší než 1.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-168">Use a value equal tooor less than 1.</span></span> <span data-ttu-id="ddc0b-169">Menší hodnoty měnit hello algoritmus méně reaktivní toosudden.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-169">Smaller values make hello algorithm less reactive toosudden changes.</span></span>
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    <span data-ttu-id="ddc0b-170">Hello hodnotu přiřazenou při aplikace hello byl spuštěn.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-170">hello value assigned when hello app has just started.</span></span> <span data-ttu-id="ddc0b-171">Není to snížit při ladění.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-171">Don't reduce this while you're debugging.</span></span> 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    <span data-ttu-id="ddc0b-172">Středníkem oddělené seznam typů, které nechcete toobe vzorků.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-172">A semi-colon delimited list of types that you do not want toobe sampled.</span></span> <span data-ttu-id="ddc0b-173">Rozpoznat typy jsou: závislost, události, výjimky, stránkové zobrazení, požadavku, trasování.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-173">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="ddc0b-174">Všechny instance hello zadané typy přenášejí; jsou odebírána data Hello typy, které nejsou zadané.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-174">All instances of hello specified types are transmitted; hello types that are not specified are sampled.</span></span>

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    <span data-ttu-id="ddc0b-175">Středníky oddělený seznam typů, které chcete toobe vzorků.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-175">A semi-colon delimited list of types that you want toobe sampled.</span></span> <span data-ttu-id="ddc0b-176">Rozpoznat typy jsou: závislost, události, výjimky, stránkové zobrazení, požadavku, trasování.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-176">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="ddc0b-177">Hello zadané typy jsou odebírána data; všechny instance z hello je vždy přenesen jiné typy.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-177">hello specified types are sampled; all instances of hello other types will always be transmitted.</span></span>


<span data-ttu-id="ddc0b-178">**tooswitch vypnout** adaptivního vzorkování, odeberte hello AdaptiveSamplingTelemetryProcessor uzlu z applicationinsights-config.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-178">**tooswitch off** adaptive sampling, remove hello AdaptiveSamplingTelemetryProcessor node from applicationinsights-config.</span></span>

### <a name="alternative-configure-adaptive-sampling-in-code"></a><span data-ttu-id="ddc0b-179">Alternativní: Konfigurace adaptivního vzorkování v kódu</span><span class="sxs-lookup"><span data-stu-id="ddc0b-179">Alternative: configure adaptive sampling in code</span></span>
<span data-ttu-id="ddc0b-180">Místo úpravě vzorkování v souboru .config hello, můžete použít kód.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-180">Instead of adjusting sampling in hello .config file, you can use code.</span></span> <span data-ttu-id="ddc0b-181">To vám umožní toospecify funkce zpětného volání, která je volána vždy, když se znovu zhodnotí hello vzorkovací frekvenci.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-181">This allows you toospecify a callback function that is invoked whenever hello sampling rate is re-evaluated.</span></span> <span data-ttu-id="ddc0b-182">Můžete to použít, například toofind na jaké vzorkovací frekvenci používá.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-182">You could use this, for example, toofind out what sampling rate is being used.</span></span>

<span data-ttu-id="ddc0b-183">Odebrat hello `AdaptiveSamplingTelemetryProcessor` ze souboru .config hello.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-183">Remove hello `AdaptiveSamplingTelemetryProcessor` node from hello .config file.</span></span>

<span data-ttu-id="ddc0b-184">*C#*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-184">*C#*</span></span>

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust hello settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report hello sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="ddc0b-185">([Další informace o telemetrie procesory](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="ddc0b-185">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a><span data-ttu-id="ddc0b-186">Vzorkování pro webové stránky v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="ddc0b-186">Sampling for web pages with JavaScript</span></span>
<span data-ttu-id="ddc0b-187">Můžete nakonfigurovat webové stránky pro – míra vzorkování z jakéhokoli serveru.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-187">You can configure web pages for fixed-rate sampling from any server.</span></span> 

<span data-ttu-id="ddc0b-188">Pokud jste [konfigurace hello webové stránky pro službu Application Insights](app-insights-javascript.md), hello fragment, kterou můžete získat z portálu služby Application Insights hello upravit.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-188">When you [configure hello web pages for Application Insights](app-insights-javascript.md), modify hello snippet that you get from hello Application Insights portal.</span></span> <span data-ttu-id="ddc0b-189">(V aplikacích ASP.NET hello fragment obvykle bude v _Layout.cshtml.)  Vložit řádek jako `samplingPercentage: 10,` před klíč instrumentace hello:</span><span class="sxs-lookup"><span data-stu-id="ddc0b-189">(In ASP.NET apps, hello snippet typically goes in _Layout.cshtml.)  Insert a line like `samplingPercentage: 10,` before hello instrumentation key:</span></span>

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

<span data-ttu-id="ddc0b-190">Procento hello vzorkování zvolte procentuální hodnotu, která je zavřít too100/N, kde N je celé číslo.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-190">For hello sampling percentage, choose a percentage that is close too100/N where N is an integer.</span></span>  <span data-ttu-id="ddc0b-191">Aktuálně vzorkování nepodporuje ostatní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-191">Currently sampling doesn't support other values.</span></span>

<span data-ttu-id="ddc0b-192">Pokud povolíte také – míra vzorkování na hello server, hello klienty a server bude synchronizovat tak, aby toto, v hledání mezi můžete procházet zobrazení související stránky a požadavky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-192">If you also enable fixed-rate sampling at hello server, hello clients and server will synchronize so that, in Search, you can  navigate between related page views and requests.</span></span>

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a><span data-ttu-id="ddc0b-193">Míry vzorkování pro weby technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ddc0b-193">Fixed-rate sampling for ASP.NET web sites</span></span>
<span data-ttu-id="ddc0b-194">Opravené míry vzorkování omezuje hello přenosy odesílané z webového serveru a webových prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-194">Fixed rate sampling reduces hello traffic sent from your web server and web browsers.</span></span> <span data-ttu-id="ddc0b-195">Na rozdíl od adaptivního vzorkování, snižuje telemetrická data s pevnou sazbou, které jste se rozhodli.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-195">Unlike adaptive sampling, it reduces telemetry at a fixed rate decided by you.</span></span> <span data-ttu-id="ddc0b-196">Také synchronizuje hello klient a server vzorkování tak, aby se zachovají související položky – například tak, aby se podíváte na zobrazení stránky ve vyhledávání, budete moci najít související požadavku.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-196">It also synchronizes hello client and server sampling so that related items are retained - for example, so that if you look at a page view in Search, you can find its related request.</span></span>

<span data-ttu-id="ddc0b-197">algoritmus vzorkování Hello zachová související položky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-197">hello sampling algorithm retains related items.</span></span> <span data-ttu-id="ddc0b-198">Pro každý požadavek HTTP událostí, ho a její související události jsou buď zrušených nebo přenosu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-198">For each HTTP request event, it and its related events are either discarded or transmitted.</span></span> 

<span data-ttu-id="ddc0b-199">V Průzkumníku metrik sazby například požadavku a výjimky počty násobí toocompensate faktor pro hello vzorkovací frekvenci, aby byly přibližně správné.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-199">In Metrics Explorer, rates such as request and exception counts are multiplied by a factor toocompensate for hello sampling rate, so that they are approximately correct.</span></span>

1. <span data-ttu-id="ddc0b-200">**Aktualizovat balíčky NuGet projektu na** toohello nejnovější *předběžné verze* verzi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-200">**Update your project's NuGet packages** toohello latest *pre-release* version of Application Insights.</span></span> <span data-ttu-id="ddc0b-201">Klikněte pravým tlačítkem na projekt hello v Průzkumníku řešení, zvolte spravovat balíčky NuGet, zkontrolujte **zahrnout předběžné verze** a vyhledejte Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-201">Right-click hello project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 
2. <span data-ttu-id="ddc0b-202">**Zakázat adaptivního vzorkování**: V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), odeberte, nebo komentář hello `AdaptiveSamplingTelemetryProcessor` uzlu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-202">**Disable adaptive sampling**: In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), remove or comment out hello `AdaptiveSamplingTelemetryProcessor` node.</span></span>
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. <span data-ttu-id="ddc0b-203">**Povolí modul – míra vzorkování hello.**</span><span class="sxs-lookup"><span data-stu-id="ddc0b-203">**Enable hello fixed-rate sampling module.**</span></span> <span data-ttu-id="ddc0b-204">Přidejte tento fragment příliš[souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="ddc0b-204">Add this snippet too[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> <span data-ttu-id="ddc0b-205">Procento hello vzorkování zvolte procentuální hodnotu, která je zavřít too100/N, kde N je celé číslo.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-205">For hello sampling percentage, choose a percentage that is close too100/N where N is an integer.</span></span>  <span data-ttu-id="ddc0b-206">Aktuálně vzorkování nepodporuje ostatní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-206">Currently sampling doesn't support other values.</span></span>
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a><span data-ttu-id="ddc0b-207">Alternativní: Povolte-míry vzorkování v serverovém kódu</span><span class="sxs-lookup"><span data-stu-id="ddc0b-207">Alternative: enable fixed-rate sampling in your server code</span></span>
<span data-ttu-id="ddc0b-208">Místo v souboru .config hello nastavení parametru vzorkování hello, můžete použít kód.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-208">Instead of setting hello sampling parameter in hello .config file, you can use code.</span></span> 

<span data-ttu-id="ddc0b-209">*C#*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-209">*C#*</span></span>

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="ddc0b-210">([Další informace o telemetrie procesory](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="ddc0b-210">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

## <a name="when-toouse-sampling"></a><span data-ttu-id="ddc0b-211">Když toouse vzorkování?</span><span class="sxs-lookup"><span data-stu-id="ddc0b-211">When toouse sampling?</span></span>
<span data-ttu-id="ddc0b-212">Pokud používáte hello 2.0.0-beta3 verze sady SDK technologie ASP.NET je automaticky povolen adaptivního vzorkování nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-212">Adaptive sampling is automatically enabled if you use hello ASP.NET SDK version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="ddc0b-213">Bez ohledu na to, jaké používáte verze sady SDK můžete přijímat vzorkování (v našem server).</span><span class="sxs-lookup"><span data-stu-id="ddc0b-213">No matter what SDK version you use, you can use ingestion sampling (at our server).</span></span>

<span data-ttu-id="ddc0b-214">Nepotřebujete vzorkování pro většinu aplikací malé a střední velikosti.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-214">You don’t need sampling for most small and medium size applications.</span></span> <span data-ttu-id="ddc0b-215">shromažďování dat pro všechny aktivity uživatele Hello nejužitečnější diagnostické informace a nejpřesnější statistiky jsou získat.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-215">hello most useful diagnostic information and most accurate statistics are obtained by collecting data on all your user activities.</span></span> 

<span data-ttu-id="ddc0b-216">hlavní výhody Hello vzorkování jsou:</span><span class="sxs-lookup"><span data-stu-id="ddc0b-216">hello main advantages of sampling are:</span></span>

* <span data-ttu-id="ddc0b-217">Aplikace Statistika vyřazuje ("omezení") dat body služby když vaše aplikace odesílá velmi vysoká míra telemetrie v krátkém časový interval.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-217">Application Insights service drops ("throttles") data points when your app sends a very high rate of telemetry in short time interval.</span></span> 
* <span data-ttu-id="ddc0b-218">tookeep v rámci hello [kvóty](app-insights-pricing.md) datových bodů pro cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-218">tookeep within hello [quota](app-insights-pricing.md) of data points for your pricing tier.</span></span> 
* <span data-ttu-id="ddc0b-219">tooreduce síťový provoz z kolekce hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-219">tooreduce network traffic from hello collection of telemetry.</span></span> 

### <a name="which-type-of-sampling-should-i-use"></a><span data-ttu-id="ddc0b-220">Jaký typ vzorkování mám použít?</span><span class="sxs-lookup"><span data-stu-id="ddc0b-220">Which type of sampling should I use?</span></span>
<span data-ttu-id="ddc0b-221">**Použijte přijímání vzorkování, pokud:**</span><span class="sxs-lookup"><span data-stu-id="ddc0b-221">**Use ingestion sampling if:**</span></span>

* <span data-ttu-id="ddc0b-222">Často projít vaše měsíční kvóta telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-222">You often go through your monthly quota of telemetry.</span></span>
* <span data-ttu-id="ddc0b-223">Používáte verzi hello SDK, která nepodporuje vzorkování – například, hello sady Java SDK nebo ASP.NET verze starší než 2.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-223">You're using a version of hello SDK that doesn't support sampling - for example, hello Java SDK or ASP.NET versions earlier than 2.</span></span>
* <span data-ttu-id="ddc0b-224">Vám mnoho telemetrie z webových prohlížečů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-224">You're getting a lot of telemetry from your users' web browsers.</span></span>

<span data-ttu-id="ddc0b-225">**Použijte – míra vzorkování, pokud:**</span><span class="sxs-lookup"><span data-stu-id="ddc0b-225">**Use fixed-rate sampling if:**</span></span>

* <span data-ttu-id="ddc0b-226">Používáte-li hello Application Insights SDK pro ASP.NET web services verze 2.0.0 nebo novější, a</span><span class="sxs-lookup"><span data-stu-id="ddc0b-226">You're using hello Application Insights SDK for ASP.NET web services version 2.0.0 or later, and</span></span>
* <span data-ttu-id="ddc0b-227">Chcete, aby synchronizovaná vzorkování mezi klientem a serverem, takže když jste příčin události v [vyhledávání](app-insights-diagnostic-search.md), mohou procházet mezi souvisejícími událostmi hello klienta a serveru, například zobrazení stránky a požadavky http.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-227">You want synchronized sampling between client and server, so that, when you're investigating events in [Search](app-insights-diagnostic-search.md), you can navigate between related events on hello client and server, such as page views and http requests.</span></span>
* <span data-ttu-id="ddc0b-228">Jste si jisti, procenta hello odpovídající vzorkování pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-228">You are confident of hello appropriate sampling percentage for your app.</span></span> <span data-ttu-id="ddc0b-229">Měl by být dostatečně vysoký tooget přesných metrik, ale níže hello rychlost, jakou překročí kvótu cenovou a hello omezení.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-229">It should be high enough tooget accurate metrics, but below hello rate that exceeds your pricing quota and hello throttling limits.</span></span> 

<span data-ttu-id="ddc0b-230">**Použijte adaptivního vzorkování:**</span><span class="sxs-lookup"><span data-stu-id="ddc0b-230">**Use adaptive sampling:**</span></span>

<span data-ttu-id="ddc0b-231">Jinak doporučujeme, abyste adaptivního vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-231">Otherwise, we recommend adaptive sampling.</span></span> <span data-ttu-id="ddc0b-232">Tato možnost je povolena ve výchozím nastavení v serveru technologie ASP.NET hello SDK verze 2.0.0-beta3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-232">This is enabled by default in hello ASP.NET server SDK, version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="ddc0b-233">Nesnižuje provoz až na určité minimální míru, takže ho nebude mít vliv na použití nízké lokality.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-233">It doesn't reduce traffic until a certain minimum rate, so it won't affect a low-use site.</span></span>

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a><span data-ttu-id="ddc0b-234">Jak zjistím, zda je vzorkování v operaci?</span><span class="sxs-lookup"><span data-stu-id="ddc0b-234">How do I know whether sampling is in operation?</span></span>
<span data-ttu-id="ddc0b-235">toodiscover hello skutečné vzorkování míru bez ohledu na to, kde bylo použito, použijte [Analytics dotazu](app-insights-analytics.md) jako je tato:</span><span class="sxs-lookup"><span data-stu-id="ddc0b-235">toodiscover hello actual sampling rate no matter where it has been applied, use an [Analytics query](app-insights-analytics.md) such as this:</span></span>

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

<span data-ttu-id="ddc0b-236">V každé uchovávají záznam, `itemCount` označuje hello původní záznamy, které představuje, rovna too1 + hello počet předchozích zrušených záznamů.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-236">In each retained record, `itemCount` indicates hello number of original records that it represents, equal too1 + hello number of previous discarded records.</span></span> 

## <a name="how-does-sampling-work"></a><span data-ttu-id="ddc0b-237">Jak funguje vzorkování?</span><span class="sxs-lookup"><span data-stu-id="ddc0b-237">How does sampling work?</span></span>
<span data-ttu-id="ddc0b-238">-Rate a adaptivního vzorkování jsou funkce hello SDK v technologii ASP.NET verze z 2.0.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-238">Fixed-rate and adaptive sampling are a feature of hello SDK in ASP.NET versions from 2.0.0 onwards.</span></span> <span data-ttu-id="ddc0b-239">Přijímání vzorkování je funkce hello služby Application Insights a může být v rámci operace, pokud hello SDK neprovádí vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-239">Ingestion sampling is a feature of hello Application Insights service, and can be in operation if hello SDK is not performing sampling.</span></span> 

<span data-ttu-id="ddc0b-240">algoritmus vzorkování Hello, rozhodne se které toodrop položky telemetrie a které těch, které jsou tookeep (jestli je v hello SDK nebo ve službě Application Insights hello).</span><span class="sxs-lookup"><span data-stu-id="ddc0b-240">hello sampling algorithm decides which telemetry items toodrop, and which ones tookeep (whether it's in hello SDK or in hello Application Insights service).</span></span> <span data-ttu-id="ddc0b-241">rozhodnutí vzorkování Hello je založeno na několik pravidel, která zaměřte toopreserve všechny body vzájemně souvisejících dat beze změn, udržování diagnostiky prostředí ve službě Application Insights, který je možné použít a spolehlivé i s menší datové sady.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-241">hello sampling decision is based on several rules that aim toopreserve all interrelated data points intact, maintaining a diagnostic experience in Application Insights that is actionable and reliable even with a reduced data set.</span></span> <span data-ttu-id="ddc0b-242">Například pokud vaše aplikace pro chybné žádosti odesílá další telemetrické položky (například výjimky a trasování, přihlášení z této žádosti), vzorkování nebude rozdělit tento požadavek a další telemetrií.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-242">For example, if for a failed request your app sends additional telemetry items (such as exception and traces logged from this request), sampling will not split this request and other telemetry.</span></span> <span data-ttu-id="ddc0b-243">Je buď udržuje nebo je zahodí všechny společně.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-243">It either keeps or drops them all together.</span></span> <span data-ttu-id="ddc0b-244">Výsledkem je když se podíváte na podrobnosti požadavku hello ve službě Application Insights, vždy uvidíte hello žádost spolu s jeho položky přidružené telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-244">As a result, when you look at hello request details in Application Insights, you can always see hello request along with its associated telemetry items.</span></span> 

<span data-ttu-id="ddc0b-245">Pro aplikace, které definují "user" (který je nejčastější webové aplikace), hello vzorkování rozhodnutí vycházet z hello hash hello id uživatele, což znamená, že všechny telemetrická pro konkrétní uživatele, je buď zachovaná, nebo vyřadit.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-245">For applications that define "user" (that is, most typical web applications), hello sampling decision is based on hello hash of hello user id, which means that all telemetry for any particular user is either preserved or dropped.</span></span> <span data-ttu-id="ddc0b-246">Pro hello typy aplikací, které nejsou definovat uživatele (například webové služby) hello vzorkování rozhodnutí podle id operace hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-246">For hello types of applications that don't define users (such as web services) hello sampling decision is based on hello operation id of hello request.</span></span> <span data-ttu-id="ddc0b-247">Pro hello telemetrie položky, které se mají ani id uživatele ani operaci nastavit (pro položky telemetrie příklad nahlásila asynchronní vláken s žádný kontext http) je nakonec vzorkování jednoduše zaznamená procento položek telemetrie každého typu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-247">Finally, for hello telemetry items that neither have user nor operation id set (for example telemetry items reported from asynchronous threads with no http context) sampling simply captures a percentage of telemetry items of each type.</span></span> 

<span data-ttu-id="ddc0b-248">Během zobrazení telemetrie back tooyou, hello hello Application Insights service upraví hello metriky ve stejné procento vzorkování, která byla použita v době hello kolekce, toocompensate pro hello chybějící datové body.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-248">When presenting telemetry back tooyou, hello Application Insights service adjusts hello metrics by hello same sampling percentage that was used at hello time of collection, toocompensate for hello missing data points.</span></span> <span data-ttu-id="ddc0b-249">Proto při prohlížení hello telemetrii ve službě Application Insights, uživatelé hello se zobrazuje statisticky správné aproximace, které jsou velmi podobné toohello reálná čísla.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-249">Hence, when looking at hello telemetry in Application Insights, hello users are seeing statistically correct approximations that are very close toohello real numbers.</span></span>

<span data-ttu-id="ddc0b-250">Hello přesnost hello aproximace do značné míry závisí na procento hello nakonfigurované vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-250">hello accuracy of hello approximation largely depends on hello configured sampling percentage.</span></span> <span data-ttu-id="ddc0b-251">Navíc hello přesnost zvyšuje pro aplikace, které zpracovávají velké množství obecně podobné požadavky od velký počet uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-251">Also, hello accuracy increases for applications that handle a large volume of generally similar requests from lots of users.</span></span> <span data-ttu-id="ddc0b-252">Na dobrý den druhé straně, pro aplikace, které nefungují s výrazném zatížení, vzorkování není potřeba, protože tyto aplikace mohou zasílat obvykle jejich telemetrických dat při zachování v rámci kvóty hello, aniž by došlo ke ztrátě dat z omezení.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-252">On hello other hand, for applications that don't work with a significant load, sampling is not needed as these applications can usually send all their telemetry while staying within hello quota, without causing data loss from throttling.</span></span> 

<span data-ttu-id="ddc0b-253">Všimněte si, že Application Insights pro tyto typy není ukázkové metriky a relací typy telemetrických dat, protože snížení hello přesnost může být vysoce nežádoucí.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-253">Note that Application Insights does not sample Metrics and Sessions telemetry types, since for these types, reduction in hello precision can be highly undesirable.</span></span> 

### <a name="adaptive-sampling"></a><span data-ttu-id="ddc0b-254">Adaptivního vzorkování</span><span class="sxs-lookup"><span data-stu-id="ddc0b-254">Adaptive sampling</span></span>
<span data-ttu-id="ddc0b-255">Adaptivního vzorkování přidá součásti tohoto monitorování hello aktuální rychlost přenosu z hello SDK a upraví hello vzorkování procento tootry toostay v rámci maximální rychlost přenosu cíl hello.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-255">Adaptive sampling adds a component that monitors hello current rate of transmission from hello SDK, and adjusts hello sampling percentage tootry toostay within hello target maximum rate.</span></span> <span data-ttu-id="ddc0b-256">Hello úpravu jsou přepočítána v pravidelných intervalech a vychází z o pohyblivý průměr hello odchozí rychlost přenosu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-256">hello adjustment is recalculated at regular intervals, and is based on a moving average of hello outgoing transmission rate.</span></span>

## <a name="sampling-and-hello-javascript-sdk"></a><span data-ttu-id="ddc0b-257">Vzorkování a hello JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="ddc0b-257">Sampling and hello JavaScript SDK</span></span>
<span data-ttu-id="ddc0b-258">Hello straně klienta (JavaScript) SDK účastní – míra vzorkování ve spojení s hello na straně serveru SDK.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-258">hello client-side (JavaScript) SDK participates in fixed-rate sampling in conjunction with hello server-side SDK.</span></span> <span data-ttu-id="ddc0b-259">Hello instrumentovány stránky pouze odesílání telemetrických dat na straně klienta z hello stejným uživatelům, pro které hello serverové provedené své rozhodnutí příliš "ukázkové ve."</span><span class="sxs-lookup"><span data-stu-id="ddc0b-259">hello instrumented pages will only send client-side telemetry from hello same users for which hello server-side made its decision too"sample in."</span></span> <span data-ttu-id="ddc0b-260">Tato logika je navrženou toomaintain integritu uživatelské relace mezi tisk klienta a serveru stranách.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-260">This logic is designed toomaintain integrity of user session across client- and server-sides.</span></span> <span data-ttu-id="ddc0b-261">V důsledku toho z libovolné položky konkrétní telemetrii ve službě Application Insights můžete najít všechny ostatní položky telemetrie pro tohoto uživatele nebo relace.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-261">As a result, from any particular telemetry item in Application Insights you can find all other telemetry items for this user or session.</span></span> 

<span data-ttu-id="ddc0b-262">*Moje klientské a serverové telemetrie nezobrazovat koordinované ukázky, jak se uvádí výše.*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-262">*My client and server-side telemetry don't show coordinated samples as you describe above.*</span></span>

* <span data-ttu-id="ddc0b-263">Povolte – míra vzorkování serveru a klienta.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-263">Verify that you enabled fixed-rate sampling both on server and client.</span></span>
* <span data-ttu-id="ddc0b-264">Ujistěte se, že verze sady SDK hello je 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-264">Make sure that hello SDK version is 2.0 or above.</span></span>
* <span data-ttu-id="ddc0b-265">Zkontrolujte, že nastavíte hello stejné vzorkování procento v hello klient i server.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-265">Check that you set hello same sampling percentage in both hello client and server.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="ddc0b-266">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="ddc0b-266">Frequently Asked Questions</span></span>
<span data-ttu-id="ddc0b-267">*Proč není vzorkování jednoduchý "shromažďování X procento každého typu telemetrie"?*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-267">*Why isn't sampling a simple "collect X percent of each telemetry type"?*</span></span>

* <span data-ttu-id="ddc0b-268">Při velmi vysokou přesnost v metriky aproximace by poskytnout tento přístup vzorkování, by rozdělit možnost toocorrelate diagnostických dat na uživatele, relace a požadavku, což je důležité pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-268">While this sampling approach would provide with a very high precision in metric approximations, it would break ability toocorrelate diagnostic data per user, session, and request, which is critical for diagnostics.</span></span> <span data-ttu-id="ddc0b-269">Proto vzorkování funguje lépe s "shromažďovat všechny telemetrie položky pro X procento uživatelů aplikace" nebo "shromažďování všech telemetrie X Procento požadavků aplikace" logiku.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-269">Therefore, sampling works better with "collect all telemetry items for X percent of app users", or "collect all telemetry for X percent of app requests" logic.</span></span> <span data-ttu-id="ddc0b-270">Pro položky telemetrie hello nejsou přidružené hello požadavků (jako je například asynchronní zpracování na pozadí) patří hello zpět je příliš "shromažďování X procento všechny položky pro každý typ telemetrie."</span><span class="sxs-lookup"><span data-stu-id="ddc0b-270">For hello telemetry items not associated with hello requests (such as background asynchronous processing), hello fall back is too"collect X percent of all items for each telemetry type."</span></span> 

<span data-ttu-id="ddc0b-271">*Časem změnit procento vzorkování hello?*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-271">*Can hello sampling percentage change over time?*</span></span>

* <span data-ttu-id="ddc0b-272">Ano, adaptivního vzorkování postupně změní hello vzorkování procento, podle hello aktuálně zjištěnými svazku hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-272">Yes, adaptive sampling gradually changes hello sampling percentage, based on hello currently observed volume of hello telemetry.</span></span>

<span data-ttu-id="ddc0b-273">*Pokud se používá – míra vzorkování, jak lze zjistit, které vzorkování procento bude fungovat hello nejvhodnější pro mojí aplikaci?*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-273">*If I use fixed-rate sampling, how do I know which sampling percentage will work hello best for my app?*</span></span>

* <span data-ttu-id="ddc0b-274">Jedním ze způsobů je toostart s adaptivního vzorkování, podívejte se, co ohodnotit vyrovná na (viz výše otázku hello), a potom přepínač toofixed – míra vzorkování pomocí tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-274">One way is toostart with adaptive sampling, find out what rate it settles on (see hello above question), and then switch toofixed-rate sampling using that rate.</span></span> 
  
    <span data-ttu-id="ddc0b-275">Jinak máte tooguess.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-275">Otherwise, you have tooguess.</span></span> <span data-ttu-id="ddc0b-276">Analýza vašeho aktuálního využití telemetrie v AI, sledovat žádné omezení, ke kterým dochází a odhad hello objem shromážděných hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-276">Analyze your current telemetry usage in AI, observe any throttling that is occurring, and estimate hello volume of hello collected telemetry.</span></span> <span data-ttu-id="ddc0b-277">Tyto tři vstupy, společně s vybranou cenovou úroveň, navrhnout, kolik můžete chtít tooreduce hello objem shromážděných hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-277">These three inputs, together with your selected pricing tier, suggest how much you might want tooreduce hello volume of hello collected telemetry.</span></span> <span data-ttu-id="ddc0b-278">Však může zvýšit počet hello vaši uživatelé nebo jiné posunutí ve svazku hello telemetrie neplatným odhad.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-278">However, an increase in hello number of your users or some other shift in hello volume of telemetry might invalidate your estimate.</span></span>

<span data-ttu-id="ddc0b-279">*Co se stane, když je možné nakonfigurovat vzorkování procento příliš nízko?*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-279">*What happens if I configure sampling percentage too low?*</span></span>

* <span data-ttu-id="ddc0b-280">Procento příliš nízkou vzorkování (over-aggressive vzorkování) snižuje hello přesnost aproximace hello, když se pokusí toocompensate hello vizualizace dat hello pro snížení objemu dat hello Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-280">Excessively low sampling percentage (over-aggressive sampling) reduces hello accuracy of hello approximations, when Application Insights attempts toocompensate hello visualization of hello data for hello data volume reduction.</span></span> <span data-ttu-id="ddc0b-281">Navíc diagnostiky prostředí může být negativně ovlivněn, jako některé z hello zřídka selhání nebo se vzorkovat lze pomalé požadavky.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-281">Also, diagnostic experience might be negatively impacted, as some of hello infrequently failing or slow requests may be sampled out.</span></span>

<span data-ttu-id="ddc0b-282">*Co se stane, pokud je možné nakonfigurovat vzorkování procento příliš vysoká.*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-282">*What happens if I configure sampling percentage too high?*</span></span>

* <span data-ttu-id="ddc0b-283">Konfigurace příliš vysoké vzorkování procento (ne agresivní dostatečně) výsledky v na nedostatečná snížení objemu hello hello shromažďovat telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-283">Configuring too high sampling percentage (not aggressive enough) results in an insufficient reduction in hello volume of hello collected telemetry.</span></span> <span data-ttu-id="ddc0b-284">Přesto můžete setkat telemetrie, které toothrottling a hello náklady na používání Application Insights může být vyšší než můžete naplánovat kvůli toooverage poplatky související s ztráty dat.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-284">You may still experience telemetry data loss related toothrottling, and hello cost of using Application Insights might be higher than you planned due toooverage charges.</span></span>

<span data-ttu-id="ddc0b-285">*Na platformách, které lze použít vzorkování?*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-285">*On what platforms can I use sampling?*</span></span>

* <span data-ttu-id="ddc0b-286">Přijímání vzorkování může dojít automaticky pro všechny telemetrie výše určité svazku, pokud hello SDK neprovádí vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-286">Ingestion sampling can occur automatically for any telemetry above a certain volume, if hello SDK is not performing sampling.</span></span> <span data-ttu-id="ddc0b-287">To bude fungovat, například pokud vaše aplikace používá serveru Java, nebo pokud používáte starší verzi hello sady SDK technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-287">This would work, for example, if your app uses a Java server, or if you are using an older version of hello ASP.NET SDK.</span></span>
* <span data-ttu-id="ddc0b-288">Pokud používáte sady SDK technologie ASP.NET verze 2.0.0 a vyšší (hostované v Azure nebo na vlastní server), můžete získat adaptivního vzorkování ve výchozím nastavení, ale můžete přepnout toofixed míry, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-288">If you're using ASP.NET SDK versions 2.0.0 and above (hosted either in Azure or on your own server), you get adaptive sampling by default, but you can switch toofixed-rate as described above.</span></span> <span data-ttu-id="ddc0b-289">S pevnou sazbou vzorkování hello prohlížeče SDK automaticky synchronizuje toosample související události.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-289">With fixed-rate sampling, hello browser SDK automatically synchronizes toosample related events.</span></span> 

<span data-ttu-id="ddc0b-290">*Existují určité výjimečných události chci, aby toosee. Načtení je po hello vzorkování modulu?*</span><span class="sxs-lookup"><span data-stu-id="ddc0b-290">*There are certain rare events I always want toosee. How can I get them past hello sampling module?*</span></span>

* <span data-ttu-id="ddc0b-291">Inicializujte samostatnou instanci TelemetryClient s novou TelemetryConfiguration (ne hello výchozí aktivní).</span><span class="sxs-lookup"><span data-stu-id="ddc0b-291">Initialize a separate instance of TelemetryClient with a new TelemetryConfiguration (not hello default Active one).</span></span> <span data-ttu-id="ddc0b-292">Pomocí této toosend výjimečných událostí.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-292">Use that toosend your rare events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ddc0b-293">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ddc0b-293">Next steps</span></span>
* <span data-ttu-id="ddc0b-294">[Filtrování](app-insights-api-filtering-sampling.md) můžete zadat další přísnou kontrolu co pošle váš SDK.</span><span class="sxs-lookup"><span data-stu-id="ddc0b-294">[Filtering](app-insights-api-filtering-sampling.md) can provide more strict control of what your SDK sends.</span></span>

