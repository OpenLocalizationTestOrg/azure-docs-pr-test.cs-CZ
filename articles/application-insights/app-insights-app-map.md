---
title: "aaaApplication mapy ve službě Azure Application Insights | Microsoft Docs"
description: "Vizuální prezentaci hello závislostí mezi součástmi aplikace označeny klíčových ukazatelů výkonu a výstrahy."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 96ab753a100ea53ec7d367e3559b6622ab6fd182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-map-in-application-insights"></a><span data-ttu-id="4082e-103">Mapa aplikace ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="4082e-103">Application Map in Application Insights</span></span>
<span data-ttu-id="4082e-104">V [Azure Application Insights](app-insights-overview.md), mapa aplikace je visual rozložení vztahů závislosti hello komponent vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4082e-104">In [Azure Application Insights](app-insights-overview.md), Application Map is a visual layout of hello dependency relationships of your application components.</span></span> <span data-ttu-id="4082e-105">Jednotlivé komponenty zobrazuje klíčových ukazatelů výkonu například zatížení, výkonu, chyb a upozornění, toohelp že zjistit všechny součásti, která způsobila problém výkonu nebo selhání.</span><span class="sxs-lookup"><span data-stu-id="4082e-105">Each component shows KPIs such as load, performance, failures, and alerts, toohelp you discover any component causing a performance issue or failure.</span></span> <span data-ttu-id="4082e-106">Můžete kliknout na prostřednictvím z jakékoli součásti toomore podrobné diagnostiky, jako je například události Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4082e-106">You can click through from any component toomore detailed diagnostics, such as Application Insights events.</span></span> <span data-ttu-id="4082e-107">Pokud vaše aplikace používá služby Azure, můžete také kliknout na prostřednictvím diagnostics tooAzure, jako je například doporučení Poradce pro databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="4082e-107">If your app uses Azure services, you can also click through tooAzure diagnostics, such as SQL Database Advisor recommendations.</span></span>

<span data-ttu-id="4082e-108">Jako další grafy budete moct připnout aplikaci mapy toohello řídicí panel Azure, kde je plně funkční.</span><span class="sxs-lookup"><span data-stu-id="4082e-108">Like other charts, you can pin an application map toohello Azure dashboard, where it is fully functional.</span></span> 

## <a name="open-hello-application-map"></a><span data-ttu-id="4082e-109">Otevřete hello aplikace mapy</span><span class="sxs-lookup"><span data-stu-id="4082e-109">Open hello application map</span></span>
<span data-ttu-id="4082e-110">Otevřete hello mapy v okně Přehled hello pro vaši aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4082e-110">Open hello map from hello overview blade for your application:</span></span>

![Otevřete aplikaci mapy](./media/app-insights-app-map/01.png)

![Mapa aplikace](./media/app-insights-app-map/02.png)

<span data-ttu-id="4082e-113">Hello mapa znázorňuje:</span><span class="sxs-lookup"><span data-stu-id="4082e-113">hello map shows:</span></span>

* <span data-ttu-id="4082e-114">Testy dostupnosti</span><span class="sxs-lookup"><span data-stu-id="4082e-114">Availability tests</span></span>
* <span data-ttu-id="4082e-115">Součásti klientské strany (monitorovat pomocí hello JavaScript SDK)</span><span class="sxs-lookup"><span data-stu-id="4082e-115">Client-side component (monitored with hello JavaScript SDK)</span></span>
* <span data-ttu-id="4082e-116">Klientská součást produktu</span><span class="sxs-lookup"><span data-stu-id="4082e-116">Server-side component</span></span>
* <span data-ttu-id="4082e-117">Závislosti hello součásti klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="4082e-117">Dependencies of hello client and server components</span></span>

<span data-ttu-id="4082e-118">Můžete rozbalit nebo sbalit skupiny odkaz závislost:</span><span class="sxs-lookup"><span data-stu-id="4082e-118">You can expand and collapse dependency link groups:</span></span>

![Sbalit](./media/app-insights-app-map/03.png)

<span data-ttu-id="4082e-120">Pokud máte velký počet závislostí jednoho typu (SQL, HTTP atd.), se mohou objevit seskupené.</span><span class="sxs-lookup"><span data-stu-id="4082e-120">If you have many dependencies of one type (SQL, HTTP etc.), they may appear grouped.</span></span> 

![seskupené závislosti](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a><span data-ttu-id="4082e-122">Identifikaci problémů</span><span class="sxs-lookup"><span data-stu-id="4082e-122">Spot problems</span></span>
<span data-ttu-id="4082e-123">Každý uzel má ukazatele relevantní výkonu, jako je například hello zatížení, výkonu a selhání sazby za tuto součást.</span><span class="sxs-lookup"><span data-stu-id="4082e-123">Each node has relevant performance indicators, such as hello load, performance, and failure rates for that component.</span></span> 

<span data-ttu-id="4082e-124">Ikony upozornění upozorňují na možné problémy.</span><span class="sxs-lookup"><span data-stu-id="4082e-124">Warning icons highlight possible problems.</span></span> <span data-ttu-id="4082e-125">Oranžové upozornění znamená, že tam jsou chyby v žádostech, zobrazení stránek nebo volání závislostí.</span><span class="sxs-lookup"><span data-stu-id="4082e-125">An orange warning means there are failures in requests, page views or dependency calls.</span></span> <span data-ttu-id="4082e-126">Red se rozumí míra selhání výše 5 %.</span><span class="sxs-lookup"><span data-stu-id="4082e-126">Red means a failure rate above 5%.</span></span> <span data-ttu-id="4082e-127">Pokud chcete tooadjust tyto prahové hodnoty, otevřete panel Možnosti.</span><span class="sxs-lookup"><span data-stu-id="4082e-127">If you want tooadjust these thresholds, open Options.</span></span>

![selhání ikony](./media/app-insights-app-map/04.png)

<span data-ttu-id="4082e-129">Aktivní výstrahy také zobrazit nahoru:</span><span class="sxs-lookup"><span data-stu-id="4082e-129">Active alerts also show up:</span></span> 

![aktivní výstrahy](./media/app-insights-app-map/05.png)

<span data-ttu-id="4082e-131">Pokud používáte SQL Azure, je ikonu, která ukazuje, kdy jsou doporučení na tom, jak může zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="4082e-131">If you use SQL Azure, there's an icon that shows when there are recommendations on how you can improve performance.</span></span> 

![Azure doporučení](./media/app-insights-app-map/06.png)

<span data-ttu-id="4082e-133">Klikněte na možnost žádné ikonu tooget další podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="4082e-133">Click any icon tooget more details:</span></span>

![Azure doporučení](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a><span data-ttu-id="4082e-135">Diagnostické klikněte na tlačítko prostřednictvím</span><span class="sxs-lookup"><span data-stu-id="4082e-135">Diagnostic click through</span></span>
<span data-ttu-id="4082e-136">Každý z uzlů hello na mapě hello nabízí cílové kliknutím prostřednictvím pro diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="4082e-136">Each of hello nodes on hello map offers targeted click through for diagnostics.</span></span> <span data-ttu-id="4082e-137">Hello možnosti se liší v závislosti na typu hello hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="4082e-137">hello options vary depending on hello type of hello node.</span></span>

![Možnosti serveru](./media/app-insights-app-map/09.png)

<span data-ttu-id="4082e-139">Pro součásti, které jsou hostované v Azure hello možnosti zahrnují toothem přímé odkazy.</span><span class="sxs-lookup"><span data-stu-id="4082e-139">For components that are hosted in Azure, hello options include direct links toothem.</span></span>

## <a name="filters-and-time-range"></a><span data-ttu-id="4082e-140">Filtry a časový rozsah</span><span class="sxs-lookup"><span data-stu-id="4082e-140">Filters and time range</span></span>
<span data-ttu-id="4082e-141">Ve výchozím nastavení shrnuje hello mapy všechna data hello k dispozici pro zvolené období hello.</span><span class="sxs-lookup"><span data-stu-id="4082e-141">By default, hello map summarizes all hello data available for hello chosen time range.</span></span> <span data-ttu-id="4082e-142">Ale můžete filtrovat, názvy pouze konkrétní operaci tooinclude nebo závislosti.</span><span class="sxs-lookup"><span data-stu-id="4082e-142">But you can filter it tooinclude only specific operation names or dependencies.</span></span>

* <span data-ttu-id="4082e-143">Název operace: Jedná se o zobrazení stránky a typy požadavků na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="4082e-143">Operation name: This includes both page views and server-side request types.</span></span> <span data-ttu-id="4082e-144">Pomocí této možnosti hello mapa zobrazuje hello klíčového ukazatele výkonu na uzlu serveru nebo klientské hello pouze operacím hello vybrané.</span><span class="sxs-lookup"><span data-stu-id="4082e-144">With this option, hello map shows hello KPI on hello server/client-side node for hello selected operations only.</span></span> <span data-ttu-id="4082e-145">Zobrazuje hello závislosti volat v kontextu hello těchto konkrétních operací.</span><span class="sxs-lookup"><span data-stu-id="4082e-145">It shows hello dependencies called in hello context of those specific operations.</span></span>
* <span data-ttu-id="4082e-146">Název základní závislosti: Jedná se o hello AJAX prohlížeče závislosti a závislosti na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="4082e-146">Dependency base name: This includes hello AJAX browser dependencies and server-side dependencies.</span></span> <span data-ttu-id="4082e-147">Pokud sestavu vlastní závislosti telemetrie s hello TrackDependency API zároveň jsou zde.</span><span class="sxs-lookup"><span data-stu-id="4082e-147">If you report custom dependency telemetry with hello TrackDependency API, they also appear here.</span></span> <span data-ttu-id="4082e-148">Můžete vybrat hello tooshow závislosti na mapě hello.</span><span class="sxs-lookup"><span data-stu-id="4082e-148">You can select hello dependencies tooshow on hello map.</span></span> <span data-ttu-id="4082e-149">Tento výběr aktuálně nefiltruje požadavků na straně serveru hello nebo hello zobrazení stránky na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="4082e-149">Currently this selection does not filter hello server-side requests, or hello client-side page views.</span></span>

![Nastavení filtrů](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a><span data-ttu-id="4082e-151">Uložit filtry</span><span class="sxs-lookup"><span data-stu-id="4082e-151">Save filters</span></span>
<span data-ttu-id="4082e-152">filtry hello toosave jste použili, kódu pin hello filtrované zobrazení na [řídicí panel](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="4082e-152">toosave hello filters you have applied, pin hello filtered view onto a [dashboard](app-insights-dashboards.md).</span></span>

![Toodashboard PIN kód](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a><span data-ttu-id="4082e-154">Podokno chyby</span><span class="sxs-lookup"><span data-stu-id="4082e-154">Error pane</span></span>
<span data-ttu-id="4082e-155">Při kliknutí na uzel v mapě hello, podokně aplikace chyba se zobrazí na pravé straně hello shrnutí selhání pro tento uzel.</span><span class="sxs-lookup"><span data-stu-id="4082e-155">When you click a node in hello map, an error pane is displayed on hello right-hand side summarizing failures for that node.</span></span> <span data-ttu-id="4082e-156">Chyby jsou nejprve seskupené podle ID operace a potom seskupené podle ID problému.</span><span class="sxs-lookup"><span data-stu-id="4082e-156">Failures are grouped first by operation ID and then grouped by problem ID.</span></span>

![Podokno chyby](./media/app-insights-app-map/error-pane.png)

<span data-ttu-id="4082e-158">Kliknutím na selhání přejdete toohello poslední instance tohoto selhání.</span><span class="sxs-lookup"><span data-stu-id="4082e-158">Clicking on a failure takes you toohello most recent instance of that failure.</span></span>

## <a name="resource-health"></a><span data-ttu-id="4082e-159">Stav prostředků</span><span class="sxs-lookup"><span data-stu-id="4082e-159">Resource health</span></span>
<span data-ttu-id="4082e-160">Pro některé typy prostředků v hello horní části podokna hello chyba se zobrazí stav prostředku.</span><span class="sxs-lookup"><span data-stu-id="4082e-160">For some resource types, resource health is displayed at hello top of hello error pane.</span></span> <span data-ttu-id="4082e-161">Například kliknutím na uzel SQL se zobrazí stav databáze hello a všechny výstrahy, které mají aktivováno.</span><span class="sxs-lookup"><span data-stu-id="4082e-161">For example, clicking a SQL node will show hello database health and any alerts that have fired.</span></span>

![Stav prostředků](./media/app-insights-app-map/resource-health.png)

<span data-ttu-id="4082e-163">Můžete kliknout na hello prostředků název tooview standardní přehled metriky pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="4082e-163">You can click hello resource name tooview standard overview metrics for that resource.</span></span>

## <a name="end-to-end-system-app-maps"></a><span data-ttu-id="4082e-164">Systém začátku do konce aplikace mapy</span><span class="sxs-lookup"><span data-stu-id="4082e-164">End-to-end system app maps</span></span>

<span data-ttu-id="4082e-165">*Vyžaduje SDK verze 2.3 nebo vyšší*</span><span class="sxs-lookup"><span data-stu-id="4082e-165">*Requires SDK version 2.3 or higher*</span></span>

<span data-ttu-id="4082e-166">Pokud aplikace obsahuje několik komponenty, například k back endové službě kromě toohello webové aplikace - pak můžete zobrazit je všechny na mapě jednu integrované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4082e-166">If your application has several components - for example, a back-end service in addition toohello web app - then you can show them all on one integrated app map.</span></span>

![Nastavení filtrů](./media/app-insights-app-map/multi-component-app-map.png)

<span data-ttu-id="4082e-168">Mapa aplikace Hello vyhledá uzly serveru pomocí následujících závislostí HTTP volání mezi servery s hello nainstalované Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="4082e-168">hello app map finds server nodes by following any HTTP dependency calls made between servers with hello Application Insights SDK installed.</span></span> <span data-ttu-id="4082e-169">Každý prostředek Application Insights se předpokládá toocontain jeden server.</span><span class="sxs-lookup"><span data-stu-id="4082e-169">Each Application Insights resource is assumed toocontain one server.</span></span>

### <a name="multi-role-app-map-preview"></a><span data-ttu-id="4082e-170">Aplikace služby role mapy (preview)</span><span class="sxs-lookup"><span data-stu-id="4082e-170">Multi-role app map (preview)</span></span>

<span data-ttu-id="4082e-171">Hello preview aplikace s více role mapy funkce umožňuje mapy aplikace hello toouse s více servery odesílání dat toohello stejný prostředek Application Insights nebo klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="4082e-171">hello preview multi-role app map feature allows you toouse hello app map with multiple servers sending data toohello same Application Insights resource  / instrumentation key.</span></span> <span data-ttu-id="4082e-172">Servery v mapě hello jsou oddělených hello cloud_RoleName vlastnost telemetrie položky.</span><span class="sxs-lookup"><span data-stu-id="4082e-172">Servers in hello map are segmented by hello cloud_RoleName property on telemetry items.</span></span> <span data-ttu-id="4082e-173">Nastavit *mapování aplikace s více rolí* příliš*na* z hello verze Preview okno tooenable tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4082e-173">Set *Multi-role Application Map* too*On* from hello Previews blade tooenable this configuration.</span></span>

<span data-ttu-id="4082e-174">Tento přístup může požadovaných, v aplikaci micro-services, nebo v jiných scénářích, kde se mají události toocorrelate na více serverech v rámci jednoho prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4082e-174">This approach may be desired in a micro-services application, or in other scenarios where you want toocorrelate events across multiple servers within a single Application Insights resource.</span></span>

## <a name="video"></a><span data-ttu-id="4082e-175">Video</span><span class="sxs-lookup"><span data-stu-id="4082e-175">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a><span data-ttu-id="4082e-176">Váš názor</span><span class="sxs-lookup"><span data-stu-id="4082e-176">Feedback</span></span>
<span data-ttu-id="4082e-177">Zadejte prosím zpětnou vazbu prostřednictvím možnosti hello portálu zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="4082e-177">Please provide feedback through hello portal feedback option.</span></span>

![Obrázek MapLink-1](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a><span data-ttu-id="4082e-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4082e-179">Next steps</span></span>

* [<span data-ttu-id="4082e-180">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4082e-180">Azure portal</span></span>](https://portal.azure.com)
